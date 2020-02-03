pipeline {
	    agent any
	
	    parameters {
	         string(name: 'tomcat_dev', defaultValue: '172.31.17.98', description: 'Dev Server')
	         string(name: 'tomcat_qa', defaultValue: '172.31.19.189', description: 'QA Server')
	    }
	
	    triggers {
	         pollSCM('* * * * *')
	     }
	
	stages{
	        stage('Build'){
	            steps {
	                sh 'mvn clean package'
	            }
	            post {
	                success {
	                    echo 'Now Archiving...'
	                    archiveArtifacts artifacts: '**/target/*.war'
	                }
	            }
	        }
	
	        stage ('Deployments'){
	            parallel{
	                stage ('Deploy to Dev'){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/vprofile/target/vprofile-v1.war root@${params.tomcat_dev}:/usr/local/apache-tomcat-8.5.50/webapps"
	                    }
	                }
	
	                stage ("Deploy to QA"){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/vprofile/target/vprofile-v1.war root@${params.tomcat_prod}:/usr/local/apache-tomcat-8.5.50/webapps"
	                    }
	                }
	            }
	        }
	    }
	}
