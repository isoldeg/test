pipeline {
    agent any
    tools {
        maven 'MAVEN_HOME'
    }
    parameters {
        choice(name: 'APPLICATION', choices: ['https://1598951385705.teamwork.com/launchpad/login/projects', 'https://1598950530563.eu.teamwork.com/launchpad/login/projects'])
	      choice(name: 'BROWSER', choices: ['Chrome', 'Firefox', 'Safari', 'Edge'])
    }
    stages {
        stage('Build') {
            steps {
                git credentialsId: '35381c1b-98ac-44c2-9418-84a44fd0f177', url: 'https://isoldeteamwork@github.com/Teamwork/qa-projects-frontend.git'
		Applitools(applitoolsApiKey: 'wFPVDWiu3IhoeEfrcQU47eEfehJhOIKOWCZVVckISbE110', notifyByCompletion: true, serverURL: 'https://eyes.applitools.com') {
            	    sh "mvn -f TeamworkAutomatedTests/pom.xml clean"
        	}
	    }
	}
        stage('Test') {
            steps {
                Applitools(applitoolsApiKey: 'wFPVDWiu3IhoeEfrcQU47eEfehJhOIKOWCZVVckISbE110', notifyByCompletion: true, serverURL: 'https://eyes.applitools.com') {
                    sh "mvn -f TeamworkAutomatedTests/pom.xml test -DurlToBeTested=${params.APPLICATION} -DbrowserToBeTested=${params.BROWSER}"
		}
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying ..."
            }
        }
    }
    post {
        always {
            step([$class: 'Publisher', reportFilenamePattern: '**/testng-results.xml'])
	    emailext body: '$DEFAULT_CONTENT', subject: '$DEFAULT_SUBJECT', to: '$DEFAULT_RECIPIENTS'
	    archiveArtifacts artifacts: 'resources/screenshots/**/*.png', fingerprint: true
        }
    }
}
