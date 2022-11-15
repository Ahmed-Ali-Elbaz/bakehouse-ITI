pipeline {

    agent any
	tools {
        Docker "docker"
    }

    environment {

        registry = "ahmedhedihed/bakehouse"
        registryCredential = "dockerhub"
    }

    stages{

        stage('Build App Image') {
          steps {
            script {
              dockerImage = docker.build registry + ":$BUILD_NUMBER"
            }
          }
        }


        stage('Upload Image'){
          steps{
            script {
              docker.withRegistry('', registryCredential) {
                dockerImage.push("$BUILD_NUMBER")

              }
            }
          }
        }



      

    }


}






