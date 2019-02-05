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
            
            steps{
                script {
                app=docker.build("muddana/node-app")
                    app.inside {
                    sh 'echo $(curl http://34.218.59.30:8080)'
                    }    
                }    
           }
        }
        stage('Push Docker Image'){
          
            steps{
                script {
                    docker.withRegistry('https://registry.hub.docker.com','muddana:phone54405'){
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        } 
    }
}
