node{
   stage('SCM Checkout'){
     git 'https://github.com/Thamilselvan1997/my-app'
   }
    stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
    stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Imager'){
   sh 'docker build -t thamilselvan/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u thamilselvan -p ${dockerPassword}"
    }
   sh 'docker push thamilselvan/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 35.154.232.134:5152"
   sh "docker tag thamilselvan/myweb:0.0.2 35.154.232.134:5152/du:1.0.0"
   sh 'docker push 35.154.232.134:5152/dudu:1.0.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest thamilselvan/myweb:0.0.2' 
   }
}
}
