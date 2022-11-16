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
                        
                    """
                    
                    }

            }



        }





        // CD Stage
        stage('CD') {
            steps {

                   
                withCredentials([file(credentialsId: 'cluster', variable: 'serviceAcc')]){

                    sh """

                            gcloud auth activate-service-account --key-file="$serviceAcc"
                            gcloud container clusters get-credentials private-cluster --zone us-central1-a --project wired-sol-367809 
                            sed -i 's/tagversion/${env.BUILD_NUMBER}/g' Deployment/deploy.yaml
                            kubectl apply  -f Deployment -n jenkins

                       """

                        
                    
                }
                    
            }     

            
        }     
   

   
   
        
        
        
        
        
    }
}
