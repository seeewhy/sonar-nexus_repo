pipeline {
    agent any

    stages {   
        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
        stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        
            }
        stage('Code Qualty Scan') {
           steps {
                  withSonarQubeEnv('sonar_sever') {
             sh "sh 'mvn clean verify sonar:sonar -DskipTests"      
               }
            }
       }
        stage('Quality Gate') {
            steps {
                 waitForQualityGate abortPipeline: true
            }
        }
        stage('push to nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: 'nexus', groupId: 'SampleWebApp', nexusUrl: 'http://3.145.89.123:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0'
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
              deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://18.117.162.32:8080/')], contextPath: 'webapp', war: '**/*.war'
             
            }
            
        }
            
    }
} 
