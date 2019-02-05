pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image'){
            when {
                 branch 'master'
            }
            
            steps{
                script {
                app=docker.build("muddana/node-app")
                    app.inside {
                    sh 'echo $(curl http://34.218.59.30:8081)'
                    }    
                }    
           }
        }
        stage('Push Docker Image'){
            when {
                 branch 'master'
            }
          
            steps{
                script {
                    docker.withRegistry('https://registry.hub.docker.com','docker_hub_id'){
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        } 
    }
}
