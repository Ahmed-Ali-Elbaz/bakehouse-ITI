pipeline {
    agent any // cont-slave / ec2-slave

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





        // CD Stage
        stage('CD') {
            steps {

                   
                withCredentials([file(credentialsId: 'cluster', variable: 'kubecfg')]){

                    sh """
                            export BUILD_NUMBER=\$(cat ../build-num.txt)
                            mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                            cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                            rm -f Deployment/deploy.yaml.tmp
                            kubectl apply --kubeconfig=${kubecfg} -f Deployment

                            
                    
                    """

                        
                    
                }
                    
            }     

            
        }     
   

   
   
        
        
        
        
        
    }
}

