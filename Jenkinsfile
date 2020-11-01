pipeline {
    agent any
    tools {
        maven 'Maven 3'
        jdk 'Java 8'
    }
    options {
        buildDiscarder(logRotator(artifactNumToKeepStr: '5'))
    }
    stages {
        stage ('Build') {
            steps {
                sh 'git config --global user.email "alex@mizerak.eu" \
                && git config --global user.name "Alemiz112"'
                sh "chmod +x ./scripts/jenkinsBuild.sh && ./scripts/jenkinsBuild.sh ${BUILD_ID}"
                withMaven(options: [pipelineGraphPublisher(lifecycleThreshold: 'install')]) {
                    sh "mvn -s /root/.m2/settings.xml -version"
                    sh 'mvn clean -Pupstream -B -DSNYK_API_ENDPOINT="https://snyk.io/" -Dbuild.number=${BUILD_NUMBER} install'
                }
            }
        }

        stage('Snapshot') {
            when {
                branch "master-zlib"
            }
            steps {
                sh 'mvn source:jar deploy -DskipTests'
            }
        }
    }
    post {
        always {
            deleteDir()
        }
    }
}


