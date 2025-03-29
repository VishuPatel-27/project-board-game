pipeline {
    agent any

    tools{
        jdk 'jdk17'
        maven 'maven3'
    }
    
    environment{
        SCANNER_HOME = tool 'sonar-scanner'
    }
    
    stages {
        
        stage('Git Checkout') {
            steps {
                
               // checkout the git repository using credentials
               git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/VishuPatel-27/project-board-game.git'
            }
        }
        
        stage('Compile') {
            steps {
                // compile the code
                sh "mvn compile"
            }
        }

        stage('Test') {
            steps {
                sh "mvn test"
            }
        }
        
        stage('File System Scanning') {
            steps {
                // trivvy will scan the codebase for vulnerabilities
                //  it will generate the report in html format
                // . is the current path which want to scan
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        
        stage('SonarQube Analysis') {
         steps {
                // sonar (tool) is configuration name
                // which we already configured under the system settings
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=board-game -Dsonar.projectKey=board-game \
                            -Dsonar.java.binaries=.'''
                }
            }

        }
        
        stage("Quality Gate"){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
        
        stage ("build"){
            // build the application package
            steps{
                sh "mvn package"
            }
        }
        
        stage ("Publish artifacts to Nexus"){
            steps{
                withMaven(globalMavenSettingsConfig: 'global-maven-settings', jdk: 'jdk17', maven: 'maven3', mavenSettingsConfig: '', traceability: true) {
                    sh "mvn deploy"
                }
            }
        }       
        
        stage ("Build and Tag Docker Image"){
            steps{
                script{
                  // This step should not normally be used in your script. Consult the inline help for details.
                  withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                    sh "docker build -t imv27/board-game:latest ."
                    
                  }
                }
            }
        }  
        
        stage ("Docker Image Scan"){
            steps{
                sh "trivy image --format table -o trivy-image-report.html imv27/board-game:latest"
            }
        }  
        
        stage ("Push Docker Image"){
            steps{
                script{
                  // This step should not normally be used in your script. Consult the inline help for details.
                  withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                    sh "docker push imv27/board-game:latest"
                    
                  }
                }
            }
        }  
        
        stage ("Deploy to K8s"){
            steps{
               withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8s-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://172.31.18.169:6443') {
                    sh "kubectl apply -f k8s-deployment.yaml"
                }
            }
        }  
        
        stage ("Verify the Deployment"){
            steps{
               withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8s-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://172.31.18.169:6443') {
                    sh "kubectl get pods -n webapps"
                    sh "kubectl get svc -n webapps"
                }
            }
        }   
    
        
    }
    
    // sending an email with reports
    post {
        always {
            script {
                def jobName = env.JOB_NAME
                def buildNumber = env.BUILD_NUMBER
                def pipelineStatus = currentBuild.result ?: 'UNKNOWN'
                def bannerColor = pipelineStatus.toUpperCase() == 'SUCCESS' ? 'green' : 'red'
    
                def body = """
                    <html>
                    <body>
                    <div style="border: 4px solid ${bannerColor}; padding: 10px;">
                    <h2>${jobName} - Build ${buildNumber}</h2>
                    <div style="background-color: ${bannerColor}; padding: 10px;">
                    <h3 style="color: white;">Pipeline Status: ${pipelineStatus.toUpperCase()}</h3>
                    </div>
                    <p>Check the <a href="${BUILD_URL}">console output</a>.</p>
                    </div>
                    </body>
                    </html>
                """
    
                emailext (
                    subject: "${jobName} - Build ${buildNumber} - ${pipelineStatus.toUpperCase()}",
                    body: body,
                    to: 'vjp2741.patidar@gmail.com',
                    from: 'jenkins@example.com',
                    replyTo: 'jenkins@example.com',
                    mimeType: 'text/html',
                    attachmentsPattern: 'trivy-image-report.html'
                )
            }
        }
    }

}
