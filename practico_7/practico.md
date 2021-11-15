![Screenshot from 2021-11-15 14-43-58](https://user-images.githubusercontent.com/48757813/141835807-534c9fe2-a3e0-4778-9e58-d04f3f5ee129.png)



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
