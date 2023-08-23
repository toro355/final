
pipeline{
    agent any

    environment {
        DOCKERNAME = 'apppy-web-1'
        IMAGENAME = 'sleep10'
    }

    stages {
        stage('check image') {
            steps {
                script{
                    def statusCode=sh(script: "docker inspect '$IMAGENAME'", returnStatus:true)
                    if(statusCode == 1){
                        echo 'Building Image'
                        sh("docker build -t $IMAGENAME .")
                    } else {
                        echo "Image already exist, no build needed."
                    }
                }
            }
        }
        stage('check if container already exist') {
            steps{
                script{
                    def statusCode=sh(script: "docker ps -a --format \"table {{.Names}}\" | grep -w $DOCKERNAME", returnStatus:true)
                    if(statusCode == 0){
                        echo "Container name: $DOCKERNAME already exist. erasing the current one."
                        sh """docker rm -f $DOCKERNAME"""
                    }   
                }
            }
        }
        stage('Run container'){
            steps {
                echo 'env.DOCKERNAME'
                sh """docker run -d -p 1919:8080 --name $DOCKERNAME $IMAGENAME"""
            }
        }

        stage('12 seconds sleep before check'){
            steps{
               sleep(time:12,unit:"SECONDS")
            }
        }

        stage('check if container is down') {
            steps{
                script{
                    def statusCode=sh(script: "docker ps --format \"table {{.Names}}\" | grep -w $DOCKERNAME", returnStatus:true)
                    if(statusCode == 0){
                        echo "Container: $DOCKERNAME is still alive. Test failed"
                    }
                }

            }
        }
    }
 }
