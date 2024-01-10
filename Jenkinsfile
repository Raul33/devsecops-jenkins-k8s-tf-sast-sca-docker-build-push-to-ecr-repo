pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=oniricobuggywebapp -Dsonar.organization=oniricobuggywebapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=1d29001b387151ee23f1d509339774ea3d1cc1ab'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://992382654067.dkr.ecr.eu-west-1.amazonaws.com', 'ecr:eu-west-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
