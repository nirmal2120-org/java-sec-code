pipeline {
    agent any

    environment {
        // Ensure this path actually exists on your Jenkins server
        JAVA_HOME = '/usr/lib/jvm/java-25-openjdk'
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
        POLARIS_TOKEN = credentials('POLARIS_TOKEN')
    }

    stages {
        stage('Verify Java') {
            steps {
                sh 'echo JAVA_HOME=$JAVA_HOME'
                sh 'java -version'
            }
        }

        stage('Build Application') {
            steps {
                // FIX: Grant execution permission to the Maven wrapper
                sh 'chmod +x mvnw'
                sh './mvnw clean install -DskipTests'
            }
        }

        stage('Download Polaris Bridge CLI') {
            steps {
                sh '''
                    # Updated to the official Synopsys Bridge URL
                    curl -L -o bridge.zip https://synopsys.com
                    unzip -o bridge.zip
                    chmod +x synopsys-bridge*/synopsys-bridge
                '''
            }
        }

        stage('Run Polaris Scan') {
            steps {
                sh '''
                    # Updated syntax for the current Synopsys Bridge CLI
                    ./synopsys-bridge*/synopsys-bridge \
                    --stage polaris \
                    polaris.serverUrl=https://poc.blackduck.com \
                    polaris.accessToken=$POLARIS_TOKEN \
                    polaris.assessment.types=SAST,SCA
                '''
            }
        }
    }
}
