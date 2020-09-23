pipeline {
     environment { 
        registry = "anrad13/tomcatsamplewebapp" 
        registryCredential = 'dockerhub_id' 
        dockerImage = '' 
    }
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

        stage('Create Tomcat Docker Image and push it to github'){
            steps {
                sh "docker build . -t anrad13/tomcatsamplewebapp:${env.BUILD_ID}"
                //sh "docker login --username=user --password=password
                //sh "docker push anrad13/tomcatsamplewebapp:${env.BUILD_ID}"
                    
            }
        }
        
        stage('Building our image') { 
            steps { 
                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }
        stage('Deploy our image') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                } 
            }
        } 
        
    }
}
