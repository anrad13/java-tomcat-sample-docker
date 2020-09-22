pipeline {
    agent any
    stages {
        stage('Build Application') {
            steps {
                sh 'mvn -f pom.xml clean package'
            }
            post {
                success {
                    echo "Now Archiving the Artifacts...."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage('Create Tomcat Docker Image'){
            steps {
                sh "pwd"
                sh "ls -a"
                sh "docker build . -t anrad13/tomcatsamplewebapp:${env.BUILD_ID}"
            }
        }
        stage('Push Tomcat Docker Image to Docker Hub'){
            steps {
                sh "docker push anrad13/tomcatsamplewebapp:${env.BUILD_ID}"
            }
        }
        
    }
}
