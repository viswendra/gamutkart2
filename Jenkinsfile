pipeline {
    agent any

//	tools {
//		maven 'maven3.6'
//	}
//	environment {
//		M2_INSTALL = "/usr/bin/mvn"
//	}

    stages {
        stage('Clone-Repo') {
	    steps {
	        checkout scm
	    }
        }

        stage('Build') {
            steps {
                sh 'mvn install -Dmaven.test.skip=true'
            }
        }
		
        stage('Unit Tests') {
            steps {
                sh 'mvn compiler:testCompile'
                sh 'mvn surefire:test'
                junit 'target/**/*.xml'
            }
        }

        stage('Deployment') {
            steps {
                sh 'sshpass -p "gamut" scp target/gamutgurus.war gamut@172.17.0.3:/home/gamut/distoss/apache-tomcat-9.0.63/webapps'
                sh 'sshpass -p "gamut" ssh gamut@172.17.0.3 "/home/gamut/distoss/apache-tomcat-9.0.63/bin/startup.sh"'
            }
        }
       stage('Notification') {
           steps{
           emailext body: 'if something is broken you are responsible ,hence fix it ASAP',
                 subject: 'Build status on jenkinsfile', to: 'tammalivishu@gmail.com'
            }
        }
    }
}
