pipeline {
    agent any

	parameters {
        string(name: 'GIT_URL', defaultValue: 'https://github.com/matthcol/movieapijava2021')
		string(name: 'GIT_BRANCH', defaultValue: 'master')
    }

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
		jdk "JDK11"
    }

    stages {
		stage('Compile') {
			steps {
                // Get some code from a GitHub repository
                git branch: "${params.GIT_BRANCH}",
					url: "${params.GIT_URL}"

                // Run Maven on a Unix agent.
                sh "mvn clean compile"
			}
		}
        stage('Test') {
            steps {
                sh "mvn test"
				junit '**/target/surefire-reports/TEST-*.xml'
            }
		}
		stage('Package') {
            steps {
				sh "mvn -DskipTests package"
			}
			post {
                success {
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}
