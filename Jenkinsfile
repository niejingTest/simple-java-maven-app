pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
					always {
					  step([$class: 'Mailer',
						notifyEveryUnstableBuild: true,
						recipients: "myname@gmail.com",
						sendToIndividuals: true])
						
						step(
						 junit 'target/surefire-reports/*.xml'	
						)
					}
				}               
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
			
        }
    }
}