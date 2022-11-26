pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh 'chmod +x ./gradlew'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        
    stage('DeployToStaging') {
            
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'webserver_login', keyFileVariable: '')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'Staging', 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'rm -rf /opt/train-schedule/* && sudo unzip /tmp/trainSchedule.zip -d /opt/train-schedule'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}
