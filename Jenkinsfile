pipeline {
    agent {label "slave"}  // cont-slave / ec2-slave

    stages {

        // CI Stage
        stage('Ci') {
            steps {

                script {

                    if ( env.BRANCH_NAME == "release" ) {
                    
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





        // CD Stage
        stage('CD') {
            steps {

                script {    

                    if ( env.BRANCH_NAME == "dev" || env.BRANCH_NAME == "test" || env.BRANCH_NAME == "prod" ) {   
                   
                        withCredentials([file(credentialsId: 'cluster', variable: 'kubecfg')]){

                            sh """
                                    export BUILD_NUMBER=\$(cat ../build-num.txt)
                                    cat Deployment/deploy.yaml | envsubst > Deployment/deploy.yaml
                                    mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                                    rm -f Deployment/deploy.yaml.tmp
                                    kubectl apply --kubeconfig=${kubecfg} -f Deployment
        
                                    
                            
                            """

                                
                            
                        }
                    }    
                 
                }
                 
            }     

            
        }         
   

   
   
        
        
        
        
        
    }
}

