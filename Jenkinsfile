pipeline {
    environment {
        dockerhubCredentials = 'dockerhubCredentials'
    }
    agent any
    stages {
        stage('Check branch & Hash image') {
            steps {
                script {
                    sh "echo 'Deploy to staging'"
                    script {
                        if(env.BRANCH_NAME=='master'){
                            sh"echo 'your build can start'"
                            script {
                                env.GIT_HASH = sh(
                                    script: "git show --oneline | head -1 | cut -d' ' -f1",
                                    returnStdout: true
                                ).trim()
                            }
                        }else if(env.BRANCH_NAME=='develop'){  
                            sh"eho 'Please create a pull request and have a successful merge'" 
                        }
                    }
                }
            }
        }
        stage('Lint Dockerfile') {
            steps {
                script {
                    docker.image('hadolint/hadolint:latest-debian').inside() {
                            sh 'hadolint ./Dockerfile | tee -a hadolint_lint.txt'
                            sh '''
                                lintErrors=$(stat --printf="%s"  hadolint_lint.txt)
                                if [ "$lintErrors" -gt "0" ]; then
                                    echo "Errors have been found, please see below"
                                    cat hadolint_lint.txt
                                    exit 1
                                else
                                    echo "There are no erros found on Dockerfile!!"
                                fi
                            '''
                    }
                }
            }
        }
        stage('Build & Push to dockerhub') {
            steps {
                script {
                    dockerImage = docker.build("tabiidris/capstone-bcrypt:${env.GIT_HASH}")
                    docker.withRegistry('', dockerhubCredentials) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Scan Dockerfile to find vulnerabilities') {
            steps{
                aquaMicroscanner imageName: "tabiidris/capstone-bcrypt:${env.GIT_HASH}", notCompliesCmd: 'exit 4', onDisallowed: 'fail', outputFormat: 'html'
            }
        }
        stage('Deploying to EKS') {
            steps {
                dir('k8s') {
                    withAWS(credentials: 'eksCredentials', region: 'us-west-2') {
                        sh "/usr/local/bin/aws eks --region us-west-2 update-kubeconfig --name capstoneCluster-G3Tof64MEiBF"
                        sh 'kubectl apply -f capstone-k8s.yaml'
                    }
                }
            }
            
        }
        stage("Cleaning Docker up") {
            steps {
                script {
                    sh "echo 'Cleaning Docker up'"
                    sh "docker system prune"
                }
            }
        }
    }
}