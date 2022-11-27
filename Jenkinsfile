pipeline {
    agent {
        docker {
            image 'maven:3.8.1-adoptopenjdk-11' 
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
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
		stage('lock-resource'){

            steps{

                echo 'Starting resource locking'	

                lock(label: 'label', variable: 'var', resource: null) {
                    echo "Resource locked: ${env.var}"
                }
                echo 'Finished resources locking'

            }

        }
    }
}
