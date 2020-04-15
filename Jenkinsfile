pipeline{
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    tools {
        maven 'maven'
    }
    
    stages{
        stage("Source Control -Git Checkout"){
            steps{
                git url: 'https://github.com/raju-ranjan/hello-app.git'
            }
        }
        
	
         stage("Compile - Maven") {
           
            steps {
                          
                sh 'mvn -f pom.xml package'            
            }
        }   
        
       stage('SSH transfer') {
			steps {
			sshPublisher(
		   continueOnError: false, failOnError: true,
		   publishers: [
			sshPublisherDesc(
			 configName: "Ansible",
			 verbose: true,
			 transfers: [
			  sshTransfer(
			   sourceFiles: "webapp/target/*.war",
			   removePrefix: "webapp/target/",
			   remoteDirectory: "//opt/docker",
			   execCommand: "ansible-playbook -i /opt/docker/hosts /opt/docker/docker-create-push-webapp.yml --limit localhost && ansible-playbook -i /opt/docker/hosts /opt/docker/docker-pull-run-webapp.yml --limit 18.220.175.18"
			   
      )
     ])
   ])
 }
}  
 
    }
}
