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
    
    stage('$SERVICE') {
            git branch: 'master',
                url: 'https://github.com/mustafaguven/$SERVICE.git'
            sh "mvn clean install -e"
    }
    
    stage('docker build') {
        sh "docker build -f Dockerfile -t $SERVICE . "
    }
    
     stage('docker push') {
        withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'docker_user_name', passwordVariable: 'docker_user_pass')]){
           sh "docker login -u  $docker_user_name -p  $docker_user_pass"
           sh "docker tag $SERVICE:latest $docker_user_name/$SERVICE:latest"
           sh "docker push $docker_user_name/$SERVICE"
            }
     }
   
    stage('docker remove unused image') {
        sh "docker rmi $SERVICE -f"
    }
}
