pipeline { 
    agent {
        docker {
            image 'maven:3.9.0'
            args '-v /root/.m2:/root/.m2'
        }
    }
    
    environment {
        GITHUB_URL = "https://github.com/pasqueque/jenkins-docker-maven-java-webapp.git"
    }

    stages {
        stage('Check out') {
            steps {
                git branch: 'master', changelog: false, poll: false, url: env.GITHUB_URL
            }
        }
          
        stage('Build by Maven Package') {
            steps {
                sh 'mvn clean package'
            }
            
        }
        
        stage('Build Docker OWN image') {
            steps {
                dir ('.') {
                    sh "docker build -t pasqueque/pasqueque-repo:${BUILD_TAG} ."
                }
                //sh 'whoami'
            }
            
        }
        
        stage('Push Image to Docker HUB') {
            steps {  
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                    docker login -u $USERNAME -p $PASSWORD
                    """
                }
               
               sh "docker push pasqueque/pasqueque-repo:${BUILD_TAG}"
            }
            
        }
    }
}
        /*
        stage('Deploy webAPP in DEV Env') {
            steps {
                sh 'sudo docker rm -f myjavaapp'
                sh "sudo docker run  -d  -p  8080:8080 --name myjavaapp   vimal13/javaweb:${BUILD_TAG}"
                //sh 'whoami'
            }
            
        }
        
        
        stage('Deploy webAPP in QA/Test Env') {
            steps {
               
               sshagent(['QA_ENV_SSH_CRED']) {
    
                    sh "ssh  -o  StrictHostKeyChecking=no ec2-user@13.233.100.238 sudo docker rm -f myjavaapp"
                    sh "ssh ec2-user@13.233.100.238 sudo docker run  -d  -p  8080:8080 --name myjavaapp   vimal13/javaweb:${BUILD_TAG}"
                }

            }
            
        }
        
        
         stage('QAT Test') {
            steps {
                
               // sh 'curl --silent http://13.233.100.238:8080/java-web-app/ |  grep India'
                
                retry(10) {
                    sh 'curl --silent http://13.233.100.238:8080/java-web-app/ |  grep India'
                }
            
               
            }
        }
          
        
         
         
        stage('approved') {
            steps {
                
            
            script {
                Boolean userInput = input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
                echo 'userInput: ' + userInput

                if(userInput == true) {
                    // do action
                } else {
                    // not do action
                    echo "Action was aborted."
                }
            
                
            }
        }
        }
        
        
         
        
        stage('Deploy webAPP in Prod Env') {
            steps {
               
               sshagent(['QA_ENV_SSH_CRED']) {
    
                    
                    sh "ssh  -o  StrictHostKeyChecking=no ec2-user@13.232.250.244 sudo kubectl  delete    deployment myjavawebapp"
                    sh "ssh  ec2-user@13.232.250.244 sudo kubectl  create    deployment myjavawebapp  --image=vimal13/javaweb:${BUILD_TAG}"
                    sh "ssh ec2-user@13.232.250.244 sudo wget https://raw.githubusercontent.com/vimallinuxworld13/jenkins-docker-maven-java-webapp/master/webappsvc.yml"
                    sh "ssh ec2-user@13.232.250.244 sudo kubectl  apply -f webappsvc.yml"
                    sh "ssh ec2-user@13.232.250.244 sudo kubectl  scale deployment myjavawebapp --replicas=5"
                }

            }
            
        } 
        
    
        
    }
    
  
        
     post {
         always {
             echo "You can always see me"
         }
         success {
              echo "I am running because the job ran successfully"
         }
         unstable {
              echo "Gear up ! The build is unstable. Try fix it"
         }
         failure {
             echo "OMG ! The build failed"
             mail bcc: '', body: 'hi check this ..', cc: '', from: '', replyTo: '', subject: 'job ete fail', to: 'vdaga@lwindia.com'
         }
     }
    
    
    
}
*/