pipeline {
    agent any
    tools {
        maven "MAVEN3"
        jdk "openjdk-8"
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
        SETTINGS_XML_PATH = '/path/to/settings.xml'
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
    }
}
