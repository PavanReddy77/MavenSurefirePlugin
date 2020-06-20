pipeline
{
	agent any
	stages
	{
		stage('Git Checkout')
		{
			steps
			{
				git 'https://github.com/PavanReddy77/MavenSurefirePlugin.git'
			}
		}
		stage('SonarQube Analysis')
		{
			steps
			{
				echo "SonarQube Test is Started"
				bat 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dlicense.skip=true'
				echo "SonarQube Test is Completed"
			}
		}
		stage('Build')
		{
			steps
			{
				echo "Build is Started"
				bat "mvn clean package"
				echo "Build is Successful"
			}
		}
		stage('Deploy')
		{
			steps
			{
				echo "Deployment of Code is Started"
				echo "Deployment of Code is Successful"
			}
		}
		stage('Chrome Tests')
		{
			parallel
			{
				stage('Smoke Suite')
				{
					steps
					{
						echo "Smoke Test Execution is Started"
						bat "mvn test -PSmoke"
						echo "Smoke Test Execution is Successful"
					}
				}
				stage('Regression Suite')
				{
					steps
					{
						echo "Regression Test Execution is Started"
						bat "mvn test -PRegression"
						echo "Regression Test Execution is Successful"
					}
				}
			}
		}
		stage('Release')
		{
			steps
			{
				echo "Release is Started"
				echo "Release is Successful"
			}
		}
		stage('Notifications')
		{
			parallel
			{
				stage('Slack')
				{
					steps
					{
						slackSend channel: 'test-automation',
						color: 'good',
						message: "*${currentBuild.currentResult}:* Job Name: ${env.JOB_NAME} || Build Number: ${env.BUILD_NUMBER}\n More information at: ${env.BUILD_URL}"
					}
				}
				stage('Gmail')
				{
					steps
					{
						emailext body: "*${currentBuild.currentResult}:* Job Name: ${env.JOB_NAME} || Build Number: ${env.BUILD_NUMBER}\n More information at: ${env.BUILD_URL}",
						subject: 'Declarative Pipeline Build Status',
						to: 'Pavankrishnan1993@gmail.com'
					}
				}
			}
		}
	}
	post
	{
        	success 
		{
           		 echo 'This Job has been Successfully Executed and Notifications have been sent to Slack and Gmail..!!'
        	}
        	unstable 
		{
           		echo 'This Job is Unstable - Notifications have been sent to Slack and Gmail..!!'
			
			slackSend channel: 'test-automation',
			color: 'good',
			message: "*${currentBuild.currentResult}:* Job Name: ${env.JOB_NAME} || Build Number: ${env.BUILD_NUMBER}\n More information at: ${env.BUILD_URL}"
        		
			emailext body: "*${currentBuild.currentResult}:* Job Name: ${env.JOB_NAME} || Build Number: ${env.BUILD_NUMBER}\n More information at: ${env.BUILD_URL}",
			subject: 'Declarative Pipeline Build Status',
			to: 'Pavankrishnan1993@gmail.com'
		}
        	failure 
		{
            		echo 'This Job is Failed - Notifications have been sent to Slack and Gmail..!!'
			
			slackSend channel: 'test-automation',
			color: 'good',
			message: "*${currentBuild.currentResult}:* Job Name: ${env.JOB_NAME} || Build Number: ${env.BUILD_NUMBER}\n More information at: ${env.BUILD_URL}"
        		
			emailext body: "*${currentBuild.currentResult}:* Job Name: ${env.JOB_NAME} || Build Number: ${env.BUILD_NUMBER}\n More information at: ${env.BUILD_URL}",
			subject: 'Declarative Pipeline Build Status',
			to: 'Pavankrishnan1993@gmail.com'
		}
	}
}
