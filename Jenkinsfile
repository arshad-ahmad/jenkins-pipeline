pipeline {
    agent any
	environment {
		PROJECT_ID = 'striped-bastion-329118'
                CLUSTER_NAME = 'cluster-1'
                LOCATION = 'us-central1-c'
                CREDENTIALS_ID = 'kubernetes'		
	}
	
    stages {
	    stage('Checkout code') {
		    steps {
			    checkout scm
		    }
	    stage('Build') {
		    steps {
			    script {
				    app = docker.build("arshad1914/pipeline:${env.BUILD_ID}")
		    	    }
		    }
	    }
	    
	    stage('Push image') {
		    steps {
			    scrip {
				    docker.withRegistry('https://registry.hub.docker.com', 'arshad1914') {
					    app.push('latest')
					    app.push("${env.BUILD_ID}")
		    }
	    }
	    
	    stage('Build Docker Image') {
		    steps {
			    sh 'whoami'
			    script {
				    myimage = docker.build("arshad1914/pipeline:${env.BUILD_ID}")
			    }
		    }
	    }
	    
	    stage('Deploy to K8s') {
		    steps{
			    echo "Deployment started ..."
			    sh 'ls -ltr'
			    sh 'pwd'
        stage('Deploy to GKE') {
            steps{
                sh "sed -i 's/pipeline:latest/pipeline:${env.BUILD_ID}/g' deployment.yaml"
                step([$class: 'KubernetesEngineBuilder', projectID: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }
    }    
}
