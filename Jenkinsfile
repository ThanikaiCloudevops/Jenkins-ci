pipeline {
    agent any
    tools{
        maven "MAVEN3"
        jdk "openjdk-8"
    }
    environment{
        SNAP_REPO = 'Projects-Snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = 'Projects-Release' 
        CENTRAL_REPO = 'Projects-Dep'
        NEXUSIP = '172.31.95.110'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'Projects-Central'
        NEXUS_LOGIN = 'nexuslogin'

    }

    stages {
        stage ('build mat')
        steps{
            sh 'mvn -s settings.xml -DskipTest install'
        }
        post {
            success {
                echo "Now Archiving"
                archiveArtifacts artifacts: '**/*.war'
            }
        }
    }
}