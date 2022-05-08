node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build image') {
  
       app = docker.build("ebenezermakinde/simplestaticapp")
    }

    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed - just a placeholder"'
        }
    }

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'manifestupdate', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}