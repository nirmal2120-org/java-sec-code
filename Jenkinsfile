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
                    unzip -q bridge.zip
                    chmod +x bridge-cli-bundle-linux64/bridge-cli
                '''
            }
        }
 
        stage('Run Polaris Scan') {
            steps {
                sh '''
                    ./bridge-cli-bundle-linux64/bridge-cli \
                    --server-url=https://poc.polaris.blackduck.com \
                    --access-token=$POLARIS_TOKEN \
                    --assessment-types=SAST,SCA
                '''
            }
        }
    }
}
