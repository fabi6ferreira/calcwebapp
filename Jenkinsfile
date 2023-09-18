pipeline {
    agent {
	    docker {
		    image 'maven'
		    args '-v $HOME/.m2:/root/.m2'
	    }
    }
    stages {
        stage('Git Checkout') {
            steps {
                git url: 'https://github.com/rchidana/calcwebapp.git'    
		            echo "Code Checked-out Successfully!!";
            }
        }
        
        stage('Package') {
            steps {
                bat 'mvn package'    
		            echo "Maven Package Goal Executed Successfully!";
            }
        }
        
        stage('JUNit Reports') {
            steps {
                    junit 'target/surefire-reports/*.xml'
		                echo "Publishing JUnit reports"
            }
        }
        
        stage('Jacoco Reports') {
            steps {
                  jacoco()
                  echo "Publishing Jacoco Code Coverage Reports";
            }
        }

	stage('SonarQube analysis') {
            steps {
		// Change this as per your Jenkins Configuration
                withSonarQubeEnv('SonarQube') {
                    sh "mvn package sonar:sonar"
		timeout(time: 1, unit: 'HOURS') {
			sh "mvn clean install"
                }
            }
        }
}
