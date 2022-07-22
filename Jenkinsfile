pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "m3"
    }

    stages {
        stage('Poll From SCM') {
            steps {
                // Get some code from a GitHub repository
                git credentialsId: 'git_token', url: 'git@github.com:sri-shenoyk/Jenkins_springboot_chat_app.git'
            }
        }
        stage('Mvn Build'){
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Unit Test'){
            steps{
                sh 'mvn test'
                junit 'target/surefire-reports/*.xml'
            }
        }
        stage('CheckStye Test') {
            steps {
                sh 'mvn checkstyle:checkstyle'
                recordIssues(tools: [checkStyle(pattern: '**/checkstyle-result.xml')])
            }
        }
        
        stage('Nexus Uploader'){
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'websocket-demo', classifier: '', file: 'target/websocket-demo-0.0.1-SNAPSHOT.jar', type: 'jar']], credentialsId: 'nexus-cred', groupId: 'websocket-demo', nexusUrl: '44.209.72.171:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '0.0.1-SNAPSHOT'
            }
        }
        
        stage('Sonar qube') {
            when {
                expression {choice  == 3}
            }
            steps {
                sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=chatapp -Dsonar.host.url=http://44.209.72.171:9000 -Dsonar.login=sqp_79cac47b2d73950e2f9dd1902458f4a24 }
