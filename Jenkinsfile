pipeline {
    agent any

    environment {
        POLARIS_TOKEN = credentials('POLARIS_TOKEN')
    }

    tools {
        // Maven stays as-is
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

        stage('Run Polaris Scan (Java 11 enforced)') {
            steps {
                withEnv([
                    "JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64",
                    "PATH=/usr/lib/jvm/java-11-openjdk-amd64/bin:$PATH"
                ]) {
                    sh '''
                        echo "=== Java & Maven versions used for Polaris ==="
                        java --version
                        mvn --version
                        echo "=============================================="

                        ./bridge-cli-bundle-linux64/bridge-cli \
                          --stage polaris \
                          polaris.serverUrl=https://poc.polaris.blackduck.com \
                          polaris.accessToken=$POLARIS_TOKEN \
                          polaris.application.name=Nirmal_SCM \
                          polaris.project.name=WebGoat \
                          polaris.branch.name=Polaris-testing \
                          polaris.assessment.types=SAST,SCA \
                          polaris.test.sca.type=SCA-PACKAGE,SCA-SIGNATURE \
                          polaris.waitForScan=true \
                          polaris.reports.sarif.create=true \
                          coverity.build.command="mvn clean install \
                            -DskipTests \
                            -DskipITs \
                            -Dmaven.test.skip=true \
                            -Dgpg.skip=true \
                            -fae"
                    '''
                }
            }
        }
    }
}
