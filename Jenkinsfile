pipeline{
  agent any
  tools {
        maven "maven3"
    }  
    stages{
        stage('Git Scm'){
           steps{
               git credentialsId: 'github_creden', url: 'https://github.com/Harsh18-56/MavenBuild.git'
           }
        }
    stage('marchieve artifacts'){
           steps{
               archiveArtifacts artifacts: '**/target/*.war', followSymlinks: false
           }
        }
    stage('deploy'){
           steps{
               withCredentials([usernameColonPassword(credentialsId: 'tomcat_creden', variable: '')]) {
                 sh "curl -v -u harsh:harsh -T /var/lib/jenkins/workspace/end-to-end-automation/target/java-example.war 'http://3.109.123.54:8181/manager/text/deploy?path=/pipeline_java'" 
                 }
                }
            }
        }
    
       post {
        failure {
            script {
                currentBuild.result = 'FAILURE'
            }
        }

        always {
            step([$class: 'Mailer',
                notifyEveryUnstableBuild: true,
                recipients: "harshkamble200002@gmail.com",
                sendToIndividuals: true])
        }
       }
    }
