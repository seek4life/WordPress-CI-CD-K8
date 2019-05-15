node{

  //Define all variables
  def project = 'wordpress_app'
  def imageVersion = 'v2.0'
  def namespace = "${namespace_gui}"
  def imageTag = "wordpress:v11.2"

  //Checkout Code from Git
  checkout scm

  //Stage 1 : Build the docker image.
        stage('Build image') {
            sh("docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD")
            sh("docker build -t nagaraj1171/${imageTag} wordpress_app/.")
        }
    //Stage 2 : Testing the code.
        stage('Testing the code') {
            sh("echo Testing......")
            sh("echo Testing Done")
        }

    //Stage 3 : Push the image to docker registry
        stage('Push image to registry') {
            sh("docker push nagaraj1171/${imageTag}")
        }

    //Stage 3 : Clean the old images
            stage('Cleaning Old docker and k8 images') {
                sh("kubectl delete -k . || true")
                sh('''docker rmi $(docker images -f 'dangling=true' -q) || true
                    docker rmi $(docker images | sed 1,2d | awk '{print "\$3"}') || true''')
            }

    //Stage 4 : Deploy Application

              stage('Deploy Application To K8 Cluster') {
                   sh("echo $namespace")
                   switch (namespace) {
                          //Roll out to Dev Environment
                          case "development":

                          sh ("kubectl create secret generic mysql --from-literal=password=${mysql_password} &>/dev/null")

                          // Create K8 Services

                          sh("kubectl apply -k .")

                          // Check for Service
                            sh("url_c=`minikube service wordpress --url`")
                            sh("kubectl get pods -o=wide")
                            sh("NAME=`minikube service list|grep wordpress |awk '{print ''\$''6}'`")

                            }
    // Stage 5 : Wait for services to be up or for 120 retries
    timeout(5) {
    waitUntil {
       script {
         def r = sh script: 'url_c=`minikube service wordpress --url`; wget --retry-connrefused --tries=120 --waitretry=1 -q $url_c -O /dev/null', returnStatus: true
         return (r == 0);
       }
    }
}
      }
  }
