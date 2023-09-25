pipeline {
    agent any
    tools { nodejs "node"}
    environment {
        imageName = "nexrpa/react-app"
        registryCredential = 'nexrpa'
        dockerImage = ''
    }
    stages {
        stage("Install Dependencies"){
            steps{
                sh 'npm install'
            }
        }

        stage("Tests"){
            steps {
                sh 'npm test'
            }
        }
        stage("Building Image"){
            steps {
                script {
                    dockerImage = docker.build imageName
                }
            }
        }
        stage("Deploy Image"){
            steps {
                script {
                    docker.withRegistry("https://registry.hub.docker.com", 'dockerhub-creds'){
                        //dockerImage.push("${env.BUILD_NUMBER}")
						dockerImage.push("latest")
                    }
                }
            }
        }
    }
	post {
		always {
		  deleteDir() /* cleanup the workspace */
		}
	}
}