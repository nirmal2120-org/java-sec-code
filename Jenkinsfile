pipeline {

    agent any
 
    environment {

        POLARIS_TOKEN = credentials('POLARIS_TOKEN')

    }
 
    stages {
 
        stage('Build Application') {

            steps {

                sh 'chmod +x mvnw'

                sh './mvnw clean install -DskipTests'

            }

        }
 
        stage('Download Polaris Bridge CLI') {

            steps {

                sh '''

                    curl -L -o bridge.zip https://blackduck.com

                    unzip -o bridge.zip

                    chmod +x bridge-cli*/bridge-cli

                '''

            }

        }
 
        stage('Run Polaris Scan') {

            steps {

                sh '''

                    ./bridge-cli*/bridge-cli \

                    --server-url=https://poc.polaris.blackduck.com \

                    --access-token=$POLARIS_TOKEN \

                    --assessment-types=SAST,SCA

                '''

            }

        }

    }

}
