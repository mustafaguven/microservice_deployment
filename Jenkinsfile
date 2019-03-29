node {
    stage('microservice_parent_pom') {
            git branch: 'master',
                url: 'https://github.com/mustafaguven/microservice_parent_pom.git'
            sh "mvn clean install"
    }
    
    stage('event_handler') {
            git branch: 'master',
                url: 'https://github.com/mustafaguven/event-handler-service.git'
            sh "mvn clean install"
    }
    
    stage('$service') {
            git branch: 'master',
                url: 'https://github.com/mustafaguven/$service.git'
            sh "mvn clean install -e"
    }
    
    stage('docker build') {
        sh "docker build -f Dockerfile -t catalog-service . "
    }
    
     stage('docker push') {
        withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'docker_user_name', passwordVariable: 'docker_user_pass')]){
           sh "docker login -u  $docker_user_name -p  $docker_user_pass"
           sh "docker tag catalog-service:latest $docker_user_name/catalog-service:latest"
           sh "docker push $docker_user_name/catalog-service"
            }
     }
   
    stage('docker remove unused image') {
        sh "docker rmi catalog-service -f"
    }
}
