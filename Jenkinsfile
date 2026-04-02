pipeline {
    agent any

    environment {
        POLARIS_TOKEN = credentials('POLARIS_TOKEN')
    }

    tools {
        // This ensures the 'mvn' command is available in the path
        maven 'maven-3.9.11' 
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
                        ./bridge-cli-bundle-linux64/bridge-cli \
                        --stage polaris \
                        polaris.serverUrl=https://poc.polaris.blackduck.com \
                        polaris.accessToken=$POLARIS_TOKEN \
                        polaris.application.name=Nirmal_SCM \
                        polaris.project.name=WebGoat \
                        polaris.branch.name=Polaris-testing \
                        polaris.assessment.types=SCA-PACKAGE,SCA-SIGNATURE \
                        polaris.waitForScan=true \
                        polaris.reports.sarif.create=true \
                        coverity.build.command="mvn clean install -DskipTests -DskipITs -Dmaven.test.skip=true -Dgpg.skip=true -fae"
                    '''
                
            }
        }


    }
}
