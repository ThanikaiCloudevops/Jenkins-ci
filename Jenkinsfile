pipeline {
    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    
     environment {
        SNAP_REPO = 'Projects-Snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = 'Projects-Release' 
        CENTRAL_REPO = 'Projects-Dep'
        NEXUSIP = '172.31.95.110'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'Projects-Central'
        NEXUS_LOGIN = 'nexuslogin'
        SETTINGS_XML_PATH = "${HOME}/.m2/settings.xml"
    }

    stages {
        stage('Build') {
            steps {
                sh "mvn -s ${SETTINGS_XML_PATH} -DskipTests install"
            }
            post {
                success {
                    echo "Now Archiving."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage('Test') {
            steps {
                sh "mvn -s ${SETTINGS_XML_PATH} test"
            }
        }

        stage('Checkstyle Analysis') {
            steps {
                sh "mvn -s ${SETTINGS_XML_PATH} checkstyle:checkstyle"
            }
        }

        stage('Sonar Analysis') {
            environment {
                scannerHome = tool "${SONARSCANNER}"
            }
            steps {
               withSonarQubeEnv("${SONARSERVER}") {
                   sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
              }
            }
        }

        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage("UploadArtifact") {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
                    groupId: 'QA',
                    version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                    repository: "${RELEASE_REPO}",
                    credentialsId: "${NEXUS_LOGIN}",
                    artifacts: [
                        [artifactId: 'vproapp',
                         classifier: '',
                         file: 'target/vprofile-v2.war',
                         type: 'war']
                    ]
                )
            }
        }
    }
}
