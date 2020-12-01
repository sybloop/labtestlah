pipeline {
	agent {
docker { image 'maven' }
}
stages {
stage ('Checkout') {
steps {
git branch:'master', url: 'https://github.com/ScaleSec/vulnado.git'
}
}
stage ('Build') {
steps {
sh 'mvn --batch-mode -V -U -e clean verify -Dsurefire.useFile=false -D maven.test.failure.ignore'
}
}
stage ('Analysis') {
steps {
sh 'mvn --batch-mode -V -U -e checkstyle:checkstyle pmd:pmd pmd:cpd findbugs:findbugs'
}
}
	            stage('OWASP DependencyCheck') {
                steps {
                    dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'Default'
                }
            }

}
post {
always {
junit testResults: '**/target/surefire-reports/TEST-*.xml'
recordIssues enabledForFailure: true, tools: [mavenConsole(), java(), javaDoc()]
recordIssues enabledForFailure: true, tool: checkStyle()
recordIssues enabledForFailure: true, tool: spotBugs(pattern:
'**/target/findbugsXml.xml')
recordIssues enabledForFailure: true, tool: cpd(pattern: '**/target/cpd.xml')
recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
}
	        success {
            dependencyCheckPublisher pattern: 'dependency-check-report.xml'
        }

}
	
	/*
	agent any
	stages {
	*/
    	/** UNCOMMENT THIS WHEN NEEDED  [SonarScanner]**/
		/*
	    stage('Headless browser test') {
	        agent {
                docker {
                    image 'python:3.8'
                }
            }
			steps {
			    sh 'python -m venv env'
			    sh 'pip install -r requirements.txt'
			    sh 'wget https://github.com/mozilla/geckodriver/releases/download/v0.25.0/geckodriver-v0.25.0-linux64.tar.gz '
                sh 'tar xzf geckodriver-v0.25.0-linux64.tar.gz'
                sh 'chmod +x geckodriver'
                sh 'mv geckodriver /usr/bin/geckodriver'
                sh 'apt update'
                sh 'apt install firefox-esr -y'
			    sh 'python -m unittest headless_test -v'
			    }
        }
	    */

        /*
        stage('SonarQube'){
                steps{
                script{
                    def scannerHome = tool 'SonarQube';
                    withSonarQubeEnv(){
                sh "${tool("SonarQube")}/bin/sonar-scanner \
                -Dsonar.projectKey==lab-project \
                -Dsonar.sources=.\
                -Dsonar.host.url=http://35.240.234.52:9000 \
                -Dsonar.login=dfc7b8dca5ca9741f72cf95e403219ef47c385a3"
                    }
                }
                }
            }	
        */
		
		
	
        /** UNCOMMENT THIS WHEN NEEDED  [OWASP]**/
        /*
            stage('OWASP DependencyCheck') {
                steps {
                    dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'Default'
                }
            }
       
    }
        */
    /** UNCOMMENT THIS WHEN NEEDED  [OWASP]**/
/*
        post {
        success {
            dependencyCheckPublisher pattern: 'dependency-check-report.xml'
        }
	
  }
	*/
}
