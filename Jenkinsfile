pipeline {
	agent {
		node {
			label 'built-in'
			customWorkspace '/mnt/tool/demo'
		}
	}
	tools {
		maven 'maven'
	}
	environment {
		tag = "$BUILD_ID"
		port = "100$BUILD_ID"
		
	}
	stages {
		stage ('SCM Checkout') {
			steps {
				git 'https://github.com/akash-aws/webapp.git' 
			}
		}
		stage ('mvn-package') {
			steps {
				sh 'mvn clean package' 
			}
		}
		stage ('docker-build') {
			steps {
				sh '''service docker start
				docker build -t akash7775/mytomcat:$tag . 
				docker container run -itd -p $port:8080 akash7775/mytomcat:$tag '''
				
			}
		}
		stage ('docker-push') {
			steps {
				withCredentials([string(credentialsId: 'dockerid', variable: 'dockerpwd')]) {
				sh "docker login -u akash7775 -p ${dockerid}"
                sh 'docker push akash7775/mytomcat:$tag'
			}
			}
		}
	}
}
