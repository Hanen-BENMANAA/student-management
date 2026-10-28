
pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven '2_HOME'
    }

    triggers {
        pollSCM('H/5 * * * *')
    }

    stages {
        stage('Commit') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Hanen-BENMANAA/student-management.git'

                sh 'git log -1 --pretty=format:"Commit: %h%nAuteur: %an%nMessage: %s"'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test unitaire') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'target/*.jar',
                             fingerprint: true
            echo 'Pipeline terminé avec succès.'
        }
        failure {
            echo 'Pipeline en échec. Consultez la console.'
        }
    }
}
