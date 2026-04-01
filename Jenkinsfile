pipeline {
    agent any
 
    environment {
        POLARIS_TOKEN = credentials('POLARIS_TOKEN')
    }
 
    stages {
        stage('Download Polaris Bridge CLI') {
            steps {
                sh '''
                    curl -fLsS -o bridge.zip \
                      https://repo.blackduck.com/bds-integrations-release/com/blackduck/integration/bridge/binaries/bridge-cli-bundle/latest/bridge-cli-bundle-linux64.zip
                    
                    unzip -qo bridge.zip
                    
                    chmod +x bridge-cli-bundle-linux64/bridge-cli
                '''
            }
        }
        
        stage('Run Polaris Scan') {
            steps {
                sh '''
                    # 1. Grant permission to the wrapper in the workspace
                    chmod +x mvnw
                    
                    # 2. Run Polaris and tell it to use the wrapper for the build
                    ./bridge-cli-bundle-linux64/bridge-cli \
                    --stage polaris \
                    polaris.serverUrl=https://poc.polaris.blackduck.com \
                    polaris.accessToken=$POLARIS_TOKEN \
                    polaris.application.name=Nirmal_SCM \
                    polaris.project.name=WebGoat \
                    polaris.branch.name=Polaris-testing \
                    polaris.assessment.types=SAST,SCA \
                    polaris.waitForScan=true \
                    coverity.build.command="./mvnw clean install -DskipTests"
                '''
            }
        }
    }
}
