     pipeline {
         agent any
         tools {
             maven 'Maven'
             jdk 'JDK_8'
         }
         options {
             buildDiscarder(logRotator(artifactNumToKeepStr: '5'))
         }
         stages {
             stage ('Build') {
                 steps {
                     sh 'git config --global user.email "crgodsey@gmail.com" \
                         && git config --global user.name "colinrgodsey"'
                     sh "chmod +x ./scripts/jenkinsBuild.sh && ./scripts/jenkinsBuild.sh ${BUILD_ID}"
                     withMaven(options: [pipelineGraphPublisher(lifecycleThreshold: 'install')]) {
                         sh "mvn -s /root/.m2/settings.xml -version"
                         sh 'mvn clean -Pupstream -B clean -DSNYK_API_ENDPOINT="https://snyk.io/" -Dbuild.number=${BUILD_NUMBER} install'
                     }
                 }

             }
         }
         post {
             always {
                 deleteDir()
             }
         }
     }
