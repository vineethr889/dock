pipeline {
    agent any
	
	environment {
    EMAIL_TO = 'bermcis6@gmail.com'
	}
	
    stages {	    
	    
	    stage('Test') {
		    steps {
			    echo 'Test'
			   // sh 'mvn test'
		    }
	    }

       stage('Sonar Qube') {
		    steps {
			    echo 'Test'
			   // sh 'mvn test'
		    }
	    }
	    
	    stage('Build Docker Image') {
		    steps {
			     script {
				     myimage = docker.build("raghukom/devops123:${env.BUILD_ID}")				
			     }
           //	sh 'docker build -t walmartpayment:1.0.0 .'
		    }
	    }
	    
	    stage("Push Docker Image") {
		    steps {
				 //sh 'echo $DOCKERCREDS_PSW | docker login -u $DOCKERCREDS_USR --password-stdin'
				 //sh 'docker push raghukom/devops:latest'
			     script {
				     echo "Push Docker Image"
				     withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'xyz', passwordVariable: 'pwd')]) {
                      sh 'docker login -u $xyz -p $pwd'
              }
				    myimage.push("${env.BUILD_ID}")			    
			     }
		    }
	    }

    }
  }
