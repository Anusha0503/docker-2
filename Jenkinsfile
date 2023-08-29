node {
          stage ('checkout'){
           git branch: 'main', credentialsId: 'gitcredentials', url: 'https://github.com/Anusha0503/docker-2.git'
 
         }
   stage ('build'){ 
        withMaven(globalMavenSettingsConfig: '', jdk: 'java', maven: 'maven', mavenSettingsConfig: '', traceability: true) {
        sh 'mvn clean package'
                  }
              }
    stage ('docker build image'){
      sh " docker build -t docker-2:v1 ."
    }
     stage ('docker tag&Push image'){
               sh " docker login -u mydocker1405 -p Password@123 "
               sh " docker tag docker-2:v1 mydocker1405/docker-2:$BUILD_NUMBER "
               sh  " docker push mydocker1405/docker-2:$BUILD_NUMBER "
     }
     stage (' deploy to k8s'){
     sshagent(['kubernetes_pem']) {
               sh " scp -o stricthostkeychecking=no docker-2_deployment.yaml ubuntu@3.90.53.233:/home/ubuntu"
               sh " scp -o stricthostkeychecking=no docker-2_service.yaml ubuntu@3.90.53.233:/home/ubuntu"        
               sh " ssh ubuntu@3.90.53.233  kubectl apply -f docker-2_deployment.yaml"
               sh " ssh ubuntu@3.90.53.233  kubectl apply -f docker-2_service.yaml"
          }
       }
     }
