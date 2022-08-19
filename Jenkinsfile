pipeline{
  agent any
 
  stages {    

   stage("Opening"){
         steps{
            //Welcome message
            script{
               sh "echo 'Welcome to Calsoft'"
}
}
}

  stage("Workspace_cleanup"){
        //Cleaning WorkSpace
        steps{
            step([$class: 'WsCleanup'])

}
}

   stage("Repo_clone"){
       //Clone repo from GitHub
      steps {
         checkout ([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/shubh9975/personal-projects.git']]])


}
}

  stage("linting and formatiing"){
    //fmt and lint
     steps{
      script{

       sh '''
           echo "Formatting the Java Code "
           echo "Linting the Java Code"
      '''

}
}
}

  stage("Code Compile"){
     steps{
      script{
       sh '''
           mvn clean package
       '''
}
}
}    
    


   stage("Image Building"){
     steps{
      script{
       sh '''
           docker build -t cto .
           docker tag  cto shubh9975/simple-app:v3.3.3
       '''
}
}
}    
    

   stage("Docker Login"){
     steps{
      script{
       sh '''
            docker login --username shubh9975 --password c65b19fc-7e5c-4553-bf79-1e878a505365
            
       '''
}
}
}    

   stage("Image Push"){
     steps{
      script{
       sh '''
           docker push shubh9975/simple-app:v3.3.3
       '''
}
}
}    
    
stage("Delete Image From Local"){
     steps{
      script{
       sh '''
           docker rmi -f cto
       '''
}
}
}    
    
 stage ('Invoke_CD_pipeline') {
     steps {
                //build job: 'microservice01-cd', parameters: [string(name: 'GIT_BRANCH_NAME', value: env.BRANCH_NAME)]
            build job: 'CD', propagate: false, wait: false, parameters: [[$class: 'StringParameterValue', name: 'BRANCH_NAME', value: "main"]]
            }
        }

}
    
    
}
}
