pipeline {
    agent any

    environment {
        MAVEN_VERSION = "3.9.12"
        MAVEN_HOME    = "${JENKINS_HOME}/maven"
        PATH          = "${JENKINS_HOME}/maven/bin:${env.PATH}"
    }

    stages {
        stage('Install Maven') {
            steps {
                sh '''
                echo "Installing Maven ${MAVEN_VERSION}"
                rm -rf "${MAVEN_HOME}"
                cd /tmp
                curl -fsSL -o maven.tar.gz https://archive.apache.org/dist/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz
                tar -xzf maven.tar.gz
                mv apache-maven-${MAVEN_VERSION} "${MAVEN_HOME}"
                mvn -version
                '''
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -Dmaven.test.failure.ignore=true'
            }
        }
    }

    post {
        success {
            junit '**/target/surefire-reports/TEST-*.xml'
            archiveArtifacts 'target/*.jar'
        }
        failure {
            echo 'Build Failed'
        }
    }
}
