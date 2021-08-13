node('JenkinsSlave'){
      def dockerImageName= '4599/javadedockerapp_$JOB_NAME:$BUILD_NUMBER'
      stage('SCM Checkout'){
         git 'https://github.com/snadimpally/java-groovy-docker.git'        
      }
      stage('Build'){
         // Get maven home path and build 
         //def mvnHome =  tool name: 'Maven-3.0.5-17', type: 'Apache'
         //test script for commit
         sh "/opt/maven/bin/mvn package -Dmaven.test.skip=true"
      }       
     
     stage ('Test'){
         //def mvnHome =  tool name: 'Maven-3.0.5-17', type: 'Apache'    
         sh "/opt/maven/bin/mvn verify; sleep 3"
      }
      
     stage('Build Docker Image'){         
           sh "docker build -t ${dockerImageName} ."
      }  
   
      stage('Publish Docker Image'){
         withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerPWD')]) {
              sh "docker login -u 4599 -p ${dockerPWD}"
         }
        sh "docker push ${dockerImageName}"
      }
      
    stage('Run Docker Image'){
            //def dockerContainerName = 'javadockerapp_$JOB_NAME_$BUILD_NUMBER'
            //def changingPermission='sudo chmod +x stopscript.sh'
            //def scriptRunner='sudo ./stopscript.sh'           
            //def dockerRun= "sudo docker run -p 8082:8080 -d --name ${dockerContainerName} ${dockerImageName}" 
            //withCredentials([string(credentialsId: 'deploymentserverpwd', variable: 'dpPWD')]) {
                  //sh "sudo cp -r stopscript.sh /home/ec2-user" 
                  //sh "${changingPermission}"
                  //sh "${scriptRunner}"
                  //sh "${dockerRun}"
                  sh "sudo su ec2-user && kubectl apply -f $workspace/manifest.yaml"
               
            }
            
      
      
      
         
  }
