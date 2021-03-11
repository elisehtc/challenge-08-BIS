pipeline {
    agent any
    
    tools  {
        maven "my-maven"
    }

    stages {
        stage("Start Sonarqube server"){
            steps{
                echo "====++++ Start SonarQube Server ++++===="
               // withCredentials([usernameColonPassword(credentialsId: 'user-password-vagrant', variable: 'USERPASS')]) {
    //sh '''
      //set +x
      sudo docker run -d --name my-sonarqube -p 9000:9000 sonarqube:lts
    //''' 
      //  }
          }
        }
      // Clone from Git
        stage("Clone App from Git"){
            steps{
                echo "====++++  Clone App from Git ++++===="
                git branch:"master", url: "https://github.com/elisehtc/challenge-08-BIS"
            }          
        }
        // Build and Unit Test (Maven/JUnit)
        stage("Build and Package"){
            steps{
                echo "====++++  Build and Unit Test (Maven/JUnit) ++++===="
                sh "mvn clean package"
            }           
        }        
         // Deploiement du WAR sur le server-staging avec Ansible
        stage("Deploy WAR on staging using Ansible"){
            steps{
                
                echo "====++++  Deploy WAR on staging using Ansible ++++===="
       
               ansiblePlaybook   credentialsId: 'ssh_on_server_staging', 
                     inventory: 'ansible/hosts', 
                      playbook: 'ansible/playbook-deploy-staging.yaml'             
            } 
        }
        stage("Stop Sonarqube server"){
            steps{
                echo "====++++ Stop SonarQube Server ++++===="
              //  withCredentials([usernameColonPassword(credentialsId: 'user-password-vagrant', variable: 'USERPASS')]) {
    //sh '''
      //set +x
      sudo docker stop my-sonarqube && sudo docker rm my-sonarqube
      //''' 
        //        }
             
            }        
        }
    }
}
