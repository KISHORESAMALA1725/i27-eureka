// Thsi Jenkins file is for Eureka Deployment

pipeline {
     agent {
          label 'k8s-slave'
     }

     // tools configured in jenkins-master
     tools {
        maven 'Maven-3.8.8'
        jdk 'JDK-17'
     }
     environment {
         APPLICATION_NAME = "eureka"
         POM_VERSION = readMavenPom().getVersion()
         POM_PACKAGING = readMavenPom().getPackaging()
        //  DOCKER_HUB = "docker.io/kishoresamala84"
        //  DOCKER_CREDS = credentials('dockerhub_creds') 
     }

     stages {
          stage ('build'){
             // This is where Build for Eureka application happens
             steps {
                 echo "Building ${env.APPLICATION_NAME} Application"
                 sh 'mvn clean package -DskipTest=true'
                 archive 'target/*.jar'
             }
          }
        stage ('SonarQube'){
           steps {
               //COde Quality needs to be implemented in this stage
               //Before we execute or write the code, make suer sonarqube-sanner plugin is installed 
               // sonar detaails are been configured in the manage jenkins > system
               echo "*******Starting Sonar Scans with Quality Gates*********"
               withSonarQubeEnv('SonarQube') {// SonarQube is the name we configured in Manage Jenkins > system > Sonarqube , it hsould match exactly
                   sh """
                      mvn sonar:sonar \
                        -Dsonar.projectKey=i27-eureka \
                        -Dsonar.host.url=http://34.21.68.203:9000 \
                        -Dsonar.login=sqa_a584d652a3aa8d42bb12ce2763d16021898ebe63
               """
               }
               timeout (time: 2, unit: 'MINUTES') {
                   waitForQualityGate abortPipeline: true
               }
             
           }
        } 
        stage ('BuildFormat') {
            steps {
               script { 
                   // Existing : i27-eureka-0.0.1-SNAPSHOT.jar
                   // Destination : i27-eureka-buildnumber-branchname.package
                 sh """
                   echo "Testing JAR Source: i27-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING}"
                   echo "Testing JAr Destination Format: i27-${env.APPLICATION_NAME}-${currentBuild.number}-${BRANCH_NAME}.${env.POM_PACKAGING}"
                 """
               }
            }
        }
        stage('Docker build') {
            steps {
                script {
                    echo "****** Building Docker image *******"
                    sh "cp ${WORKSPACE}/target/i27-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} ./.cicd"
                    sh "docker build --no-cache --build-arg JAR_SOURCE=i27-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} -t ${env.DOCKER_HUB}/${env.APPLICATION_NAME}:${GIT_COMMIT} ./.cicd/"
                    sh "docker login -u ${DOCKER_CREDS_USR} -p ${DOCKER_CREDS_PSW}"
                    echo "********* Push Image to Docker regitry ***********"
                    sh "docker push ${env.DOCKER_HUB}/${env.APPLICATION_NAME}:${GIT_COMMIT}"
                
                }
            }
        }
       
     }
}
    
