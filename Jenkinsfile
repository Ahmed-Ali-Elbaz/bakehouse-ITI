pipeline {
    agent any  // cont-slave / ec2-slave

    stages {

        
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
        
        stage('CD') {
            steps {
                    
                 // sh "docker run -d -p 3000:3000 ahmedhedihed/bakehouse:$BUILD_NUMBER"
                 
                withCredentials([file(credentialsId: 'cluster', variable: 'kubecfg')]){
                    // Change context with related namespace
                    // sh "kubectl config set-context $(kubectl config current-context)"   // --namespace=${namespace}

                      // sh 'cat $kubecfg > ~/.kube/config'
                       sh """
                            
                            kubectl apply  --config=$kubecfg -f Deployment/deploy.yaml
                            kubectl apply  --config=$kubecfg  -f Deployment/service.yaml
                            
                       
                       """

                        
                    
                }
                 
                 
                 
            }     

            
        }         
   

   
   
        
        
        
        
        
    }
}
