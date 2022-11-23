pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation on the new branch'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
    }
}
