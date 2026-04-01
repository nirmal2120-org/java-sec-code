pipeline {
    agent any
 
    environment {
        POLARIS_TOKEN = credentials('POLARIS_TOKEN')
    }

    tools {
        // Updated to match your Jenkins configuration
        maven 'maven-3.9.11' 
    }
 
    stages {
        stage('Download Polaris Bridge CLI') {
            steps {
                sh '''
                    curl -fLsS -o bridge.zip \
                      https://blackduck.com
                    
                    unzip -qo bridge.zip
                    
                    chmod +x bridge-cli-bundle-linux64/bridge-cli
                '''
            }
        }
        
        stage('Run Polaris Scan') {
            steps {
                sh '''
                    ./bridge-cli-bundle-linux64/bridge-cli \
                    --stage polaris \
                    polaris.serverUrl=https://blackduck.com \
                    polaris.accessToken=$POLARIS_TOKEN \
                    polaris.application.name=Nirmal_SCM \
                    polaris.project.name=WebGoat \
                    polaris.branch.name=Polaris-testing \
                    polaris.assessment.types=SAST,SCA \
                    polaris.waitForScan=true \
                    coverity.build.command="mvn clean install -DskipTests"
                '''
            }
        }
    }
}
