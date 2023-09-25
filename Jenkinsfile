pipeline {
    agent any
    tools { nodejs "node"}
	options {
		buildDiscarder(logRotator(numToKeepStr: '2'))
	}

    environment {
        imageName = "nexrpa/react-app"
        registryCredential = 'nexrpa'
        dockerImage = ''
		image_tag = 'latest'
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
                sh "docker build --no-cache -t ${imageName}:${image_tag} ."
            }
        }
        stage("Deploy Image"){
            steps {
                script {
                    docker.withRegistry("", 'dockerhub-creds'){
                        //dockerImage.push("${env.BUILD_NUMBER}")
						//dockerImage.push("latest")
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