pipeline{
	agent any 
	environment{
		PATH: "${PATH}:{tool name: 'Maven3', type: 'maven'}/bin"
	}
	stages{
		stage("SCM Checkout"){
			steps{
				git credentialsId: 'github', 
				url: 'https://github.com/Lopamudra10/my-webapp'
			}
		}
		stage("Maven Build"){
			steps{
			sh script: 'mvn clean package'
		}
	}
		stage("Deploy to tomcat"){
			steps{
				sshagent(['tomcat-dev']) {
    // stop tomcat
    sh "ssh -o StrickHostKeyChecking=no ec2user@172.31.37.193 /opt/tomcat8/bin/shutdown.sh"
    // copy the war file to remote tomcat
    sh "scp target/my-webapp.war ec2user@172.31.37.193:/opt/tomcat8/webapps/"
    // start tomcat
    sh "ssh ec2user@172.31.37.193 /opt/tomcat8/bin/start.sh"
}
			}
		}
	}
}
