pipeline {
agent {
docker { image 'maven' }
}
stages {
stage ('Checkout') {
steps {
git branch:'master', url: 'https://github.com/OWASP/Vulnerable-Web-Application.git'
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
-Dsonar.host.url=http://mynameisted.ml:9000 \
-Dsonar.login=a5a8459119bae70b648b17153a1dd0d386a854bb"
}
}
}
}
}
post {
always {
recordIssues enabledForFailure: true, tool: sonarQube()
}
}
}
