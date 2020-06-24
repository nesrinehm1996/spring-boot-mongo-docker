pipeline {
    agent any
	   tools {
        // Note: This should match with the tool name configured in your jenkins instance (JENKINS_URL/configureTools/)
        maven "maven" 
    }
    environment{
        DOCKER_TAG=getDockerTag()
       
    }
    stages{
	    stage("mvn build") {
            steps {
                script {
                    // If you are using Windows then you should use "bat" step
                    // Since unit testing is out of the scope we skip them
                    sh "mvn package -DskipTests=true"
                }
            }
        }
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t nesrinehm1996/spring-boot-mongo:${DOCKER_TAG}"
		  //sh "docker build -t nesrinehm1996/spring-boot-mongo ."  
            }
		
        }
	     stage('Docker push image'){
            steps{
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
          docker.withRegistry('https://registry.hub.docker.com', 'DOCKER_HUB_CREDENTIALS') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
               //withCredentials([string(credentialsId: 'DOCKER_HUB_CREDENTIALS', variable: 'DOCKER_HUB_CREDENTIALS')]) {
                 //       sh "docker login -u nesrinehm1996 -p ${DOCKER_HUB_CREDENTIALS}" 
	   //}
		
		//        sh "docker push nesrinehm1996/spring-boot-mongo "
	            
	     }
	     }

	    stage('Deploy app') {
     steps {
    
               kubernetesDeploy(
		       configs: 'springBootMongo.yml',
		       kubeconfigId: 'kub-cluster-config',
		       enableConfigSubstitution: true
		       )
	    }    
	}
    }
}
def getDockerTag() {
		def tag = sh script: 'git rev-parse HEAD', returnStdout: true
		return tag 
	}
