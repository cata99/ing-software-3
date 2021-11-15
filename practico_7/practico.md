

EJERCICIO 6 pipeline syntax
pipeline {
    agent any

    tools {
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {

                git branch: 'origin', credentialsId: 'Github-account', url: 'https://github.com/cata99/ing-software-3.git'
                dir('practico_6/proyectos/spring-boot/') {
                    sh "mvn -Dmaven.test.failure.ignore=true clean package"
                }
                
            }

            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
} 
