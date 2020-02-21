//repo-per-microservice

node{
    stage('SCM Checkout from GitHub'){
        git 'https://github.com/sushantac/scshop-eureka-discovery-service'
    }
    
    stage('MVN Package - eureka-discovery-service'){
        def mvnHome = tool name: 'Apache Maven', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        
        sh label: '', script: "\"${mvnCMD}\" clean package -f pom.xml"
    }
    
    stage('Build Docker Image - eureka-discovery-service') {
        sh label: '', script: 'docker build -t sushantac/eureka-discovery-service:0.0.1 --file Dockerfile .'
    }
    
    stage('Push to docker hub - eureka-discovery-service') {
	
	    withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPassword')]) {
            sh label: '', script: "docker login -u sushantac -p ${dockerHubPassword}"
        }

        sh label: '', script: 'docker push sushantac/eureka-discovery-service:0.0.1'
    }

    stage('Run container on Dev server'){
        try{
            sh label: '', script: 'docker stop eureka-discovery-service'
        } catch(all) {

        }
        
        try{
            sh label: '', script: 'docker rm eureka-discovery-service'
        } catch(all) {

        }

        sh label: '', script: 'docker run -d -p 8012:8012 --name eureka-discovery-service sushantac/eureka-discovery-service:0.0.1'
    }
    
}