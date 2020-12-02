pipeline {
agent {
docker { image 'maven' }
}
stages {
        stage('OWASP DependencyCheck') {
            steps {
                dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'Default'
            }
        }

  stage('Code Quality Check via SonarQube') {
steps {
script {
def scannerHome = tool 'SonarQube';
withSonarQubeEnv() {
sh "${tool("SonarQube")}/bin/sonar-scanner \
-Dsonar.projectKey=OWSAP \
-Dsonar.sources=. \
-Dsonar.host.url=http://syblopomg.mooo.com:9000/ \
-Dsonar.login=7d265d1f1fe61c957670c6e0603cf4136e7c6aef"
}
}
}
}
}
post {
always {
recordIssues enabledForFailure: true, tool: sonarQube()
}
          success {
            dependencyCheckPublisher pattern: 'dependency-check-report.xml'
        }

}
}
