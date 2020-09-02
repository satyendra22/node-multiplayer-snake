node ('Ubuntu-app-agent'){  
    def app
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }  
    
    stage('SCA'){
        build 'SCA-DependencyCheck'
    }
    
    stage('SAST'){
        build 'SAST-SNYK'
        build 'SAST-SONARQUBE'
    }

    stage('Build-and-Tag') {
    /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("satyendra22/snake")
    }
    stage('Post-to-dockerhub') {
    
     docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_id') {
            app.push("latest")
        			}
         }
    stage('SECURITY-IMAGE-SCANNER'){
        build 'Cloud-Anchore'
        build 'Cloud-Aquascanner'
    }
  
    
    stage('Pull-image-server') {
    
         sh "docker-compose down"
         sh "docker-compose up -d"	
      }
    
    stage('DAST')
        {
        build 'DAST-ZAP'
        build 'DAST-ARACHNI'
        }
 
}
