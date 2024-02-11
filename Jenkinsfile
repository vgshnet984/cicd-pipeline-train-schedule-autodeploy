pipeline {
    agent any
    environment {
        //be sure to replace "bhavukm" with your own Docker Hub username
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk-amd64'
        DOCKER_IMAGE_NAME = "ganeshbv/train-schedule"
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                //sh 'sudo chmod +x /opt/devopsadmin/.gradle/wrapper/dists/gradle-4.6-bin/4jp4stjndanmxuerzfseyb6wo/gradle-4.6/bin'
		sh 'sudo chmod +x /var/lib/jenkins/.gradle/wrapper/dists/gradle-6.0-bin/8ccdmgaih4za71r0tlxhaz33m/gradle-6.0/bin/gradle'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            steps {
                // Build the Docker image with sudo
                sh 'sudo docker build -t ${DOCKER_IMAGE_NAME} .'
                 }
        }

	 stage('Push Docker Image') {
    		steps {
        	script {
            	// Execute docker commands with sudo
            	sudo("docker login -u ganeshbv -p edurekavr registry-1.docker.io")
            	sudo("docker tag ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER} ganeshbv/${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}")
            	sudo("docker tag ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER} ganeshbv/${DOCKER_IMAGE_NAME}:latest")
            	sudo("docker push ganeshbv/${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}")
            	sudo("docker push ganeshbv/${DOCKER_IMAGE_NAME}:latest")
        	}
    	}
	}
	    
        
	stage('CANARY DEPLOYMENT') {
            steps {
		sh "chmod +x kubedeploy.sh"
                sh "./kubedeploy.sh"
            }
        }

	stage('Train-Schedule PROD DEPLOYMENT') {
            steps {
		sh "chmod +x kubedeploy-Prod.sh"
                sh "./kubedeploy-Prod.sh"
            }
        }
    }
}
