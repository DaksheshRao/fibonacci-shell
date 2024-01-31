pipeline {
  agent any
  
    parameters {
        string(name: 'docker_imageName', defaultValue: 'daksheshrao/fibonacci', description: 'The name of the Docker image')
        string(name: 'tag', defaultValue: '4.0', description: 'The tag for the Docker image')
        string(name: 'harbor_imageName', defaultValue: 'sre-training/dakshesh-fibonacci', description: 'The tag for the harbor image')
    }
  stages {
    stage('Build Docker image') {
      steps {
        script {
          def dockerImage = docker.build("${params.docker_imageName}:${params.tag}", "-f Dockerfile .")
        }
      }
    }
    stage('Push to Docker Hub') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker-dakshesh')
	  {
            docker.image("${params.docker_imageName}:${params.tag}").push()
          }
        }
      }
    }
    stage('Build and push image on harbor') {
      steps {
        script{
          def dockerImage = docker.build("${params.harbor_imageName}:${params.tag}", "-f Dockerfile .")
          docker.withRegistry('https://harbor.cubastion.net', 'harbor_sre'){
          dockerImage.push()
         } 
	}
       }
      }
    stage('Cleanup') {
      steps {
        cleanWs()
      }
    }
  }
}
