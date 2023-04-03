pipeline {
    agent any

    stages {
        stage('git checkout') {
            steps {
                echo "${params}
                git branch: 'main', credentialsId: 'github-tokens', url: 'https://github.com/chandugithubit/hr1-api'
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Tomcat Deploy - Dev') {
            steps {
                sshagent(['tomcat-dev']) {
                    sh "scp -o StrictHostKeyChecking=no target/hr1-api.war ec2-user@172.31.5.228:/opt/tomcat9/webapps/"
                    sh "ssh ec2-user@172.31.5.228 /opt/tomcat9/bin/shutdown.sh"
                    sh "ssh ec2-user@172.31.5.228 /opt/tomcat9/bin/startup.sh"
                }
            }
        }
            }
    post {
  always {
    cleanWs()
  }
}

    }
