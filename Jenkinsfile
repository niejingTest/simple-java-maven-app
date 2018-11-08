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
        stage('Test') {：
            steps {
                sh 'mvn test'
            }
            post {
				 success {
				emailext (
					subject: "'${env.JOB_NAME} [${env.BUILD_NUMBER}]' 更新正常",
					body: """
					详情：
					SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'
					状态：${env.JOB_NAME} jenkins 更新运行正常 
					URL ：${env.BUILD_URL}
					项目名称 ：${env.JOB_NAME} 
					项目更新进度：${env.BUILD_NUMBER}
					""",
					to: "niejing@anydef.com",
					recipientProviders: [[$class: 'DevelopersRecipientProvider']]
					)
                }   
				
                always {
                    junit 'target/surefire-reports/*.xml'					
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