pipeline {
    agent any
    tools {
        maven 'MAVEN_HOME'
    }
    parameters {
	choice(name: 'SUITE_XML_FILE', choices: ['testng-critical-tests.xml', 'testng.xml'])
	choice(name: 'BROWSER', choices: ['Chrome', 'Firefox', 'Safari', 'Edge'])
        choice(name: 'APPLICATION', choices: ['https://1598951385705.teamwork.com/launchpad/login/projects', 'https://1598950530563.eu.teamwork.com/launchpad/login/projects'])
	choice(name: 'SPACES_APPLICATION', choices: ['https://1598951385705.teamwork.com/launchpad/login/spaces', 'https://1598950530563.eu.teamwork.com/launchpad/login/space'])
	choice(name: 'CRM_APPLICATION', choices: ['https://1598951385705.teamwork.com/launchpad/login/crm', 'https://1598950530563.eu.teamwork.com/launchpad/login/crm'])
	choice(name: 'CHAT_APPLICATION', choices: ['https://1598951385705.teamwork.com/launchpad/login/chat', 'https://1598950530563.eu.teamwork.com/launchpad/login/chat'])
	choice(name: 'DESK_APPLICATION', choices: ['https://1598951385705.teamwork.com/launchpad/login/desk', 'https://1598950530563.eu.teamwork.com/launchpad/login/desk'])
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
			sh "mvn -f TeamworkAutomatedTests/pom.xml test -DsuiteXmlFile=${params.SUITE_XML_FILE} -DbrowserToBeTested=${params.BROWSER} -DurlToBeTested=${params.APPLICATION} -DspacesUrlToBeTested=${params.SPACES_APPLICATION} -DcrmUrlToBeTested=${params.CRM_APPLICATION} -DchatUrlToBeTested=${params.CHAT_APPLICATION} -DdeskUrlToBeTested=${params.DESK_APPLICATION}"
		}
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying..."
            }
        }
    }
    post {
        always {
            step([$class: 'Publisher', reportFilenamePattern: '**/testng-results.xml'])
	    emailext body: '$DEFAULT_CONTENT', subject: '$DEFAULT_SUBJECT', to: '$DEFAULT_RECIPIENTS'
	    archiveArtifacts 'TeamworkAutomatedTests/resources/screenshots/*'
        }
    }
}
