pipeline 
    agent any
    environment {
        DOCKN = 'finalapp'
        IMG = 'testsleep'
    }

    stages {
        stage('check and build') {
            steps {
               script {
                   def exist=sh(script: "docker inspect '$IMG'", returnStatus:true)
                   if(exist==1){
                       echo 'building image'
                       sh("docker build -t $IMG .")
                   }
                   else{
                       echo 'image already exists no need to make new one'

                   }
               }
            }
        }
        stage('run docker container')
        {
         script
         {
           sh("docker run $IMG")
         }

        }
}

