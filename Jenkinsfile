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
	    }
	    stage('Build image') {
		    steps {
			    script {
				    app = docker.build("arshad1914/pipeline:latest")
		    	    }
		    }
	    }
	    
	    stage('Push image') {
		    steps {
			    script {
				    docker.withRegistry('https://registry.hub.docker.com', 'arshad1914') {
					    app.push()
					    app.push("latest")
				    }
			    }
		    }
	    }
	 
	    stage('Deploy to K8s') {
		    steps{
			    echo "Deployment started ..."
			    sh 'ls -ltr'
			    sh 'pwd'
			    sh "sed -i 's/pipeline:latest/pipeline:${env.BUILD_ID}/g' deployment.yaml"
			    step([$class: 'KubernetesEngineBuilder', projectID: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
	    		}
        	}
    	}    
}
