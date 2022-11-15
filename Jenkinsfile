pipeline {
    agent {label "slave"}

    stages {

        // CI Stage
        stage('Ci') {
            steps {

 
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) { 
                    
                    sh """
                        
                        docker build .  -t ahmedhedihed/bakehouse:$BUILD_NUMBER
                        docker login -u ${USERNAME} -p ${PASSWORD}
                        docker push ahmedhedihed/bakehouse:$BUILD_NUMBER
                        echo ${BUILD_NUMBER} > ../build-num.txt
                        
                    """
                    
                    }

            }



        }





   

   
   
        
        
        
        
        
    }
}

