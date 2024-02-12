pipeline {
    agent any
    tools{
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
    environment{
        SNAP_REPO 
        NEXUS_USER
        NEXUS_PASS
        RELEASE_REPO
        CENTRAL_REPO
        NEXUSIP
        NEXUSPORT
        NEXUS_GRP_REPO
        NEXUS_LOGIN

    }

    stages {
        stage ('build mat')
        steps{
            sh 'mvn -s settings.xml -DskipTest install'
        }
    }
}