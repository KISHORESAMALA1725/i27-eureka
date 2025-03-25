// This Jenkins file is for Eureka Deployment

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
    }

    stages {
        stage ('Build'){
            // This is Where Build for Eureka application happens
            steps {
                echo "Building ${env.APPLICATION_NAME} Application"
                sh 'mvn clean package -DskipTests=true'
                // mvn clean package -DskipTests=true
                // mvn clean package -Dmaven.test.skip=true
                archive 'target/*.jar'
            }
        }
        stage ('Unit Tests'){
            steps {
                echo "****************** Performing Unit tests for ${env.APPLICATION_NAME} Application ******************"
                sh 'mvn test'
            }
        }
    }
}
