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
	    
	    stage('Deploy to K8s Dev') {
		    steps{
			    echo "Deployment started ..."

		    }
	    }
	    
	    stage('Deploy to K8s Test') {
          steps{
            echo "Deployment started ..."
            

          }
	    }

      stage('Deploy to K8s Prod - Approval') {
         when {
            branch 'master'
        }
          steps{
           
            script {
              timeout(time: 1, unit: 'HOURS') {
                input(id: "Deploy Gate", message: "Deploy to Prod?", ok: 'Deploy')
              }
            }
            }
	    }

       stage('Deploy to K8s Prod') {
         when {
              branch 'master'
          }
            steps{
            

        }	    
      }

    post {
        failure {
            emailext body: 'Check console output at $BUILD_URL to view the results. \n\n ${CHANGES} \n\n -------------------------------------------------- \n${BUILD_LOG, maxLines=100, escapeHtml=false}', 
                    to: "${EMAIL_TO}", 
                    subject: 'Build failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER'
        }
        always {
            echo 'always'
        }
        success {
            echo 'success'
        }
    }
  }
}
