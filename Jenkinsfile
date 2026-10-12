pipeline {
    agent any
    environment {
        MAVEN_HOME = "${WORKSPACE}/apache-maven-3.9.12"
    }
    stages {
        stage('Install Maven') {
            steps {
                sh '''
                echo "Installing Maven 3.9.12"
                curl -fsSL -o maven.tar.gz https://archive.apache.org/dist/maven/maven-3/3.9.12/binaries/apache-maven-3.9.12-bin.tar.gz
                tar -xzf maven.tar.gz
                $MAVEN_HOME/bin/mvn -version
                '''
            }
        }

        stage('Build with Maven') {
            steps {
                sh '$MAVEN_HOME/bin/mvn clean package -Dmaven.test.failure.ignore=true'
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
