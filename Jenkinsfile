pipeline {
  agent {
    label 'kubegit'
  }
  parameters {
    string(name: 'CARTS_ENV', defaultValue: 'production', description: 'The environment of carts to generate load for.', trim: true)
  }
  stages {
    stage('Generate Load to Carts Service') {
      steps {
        container("kubectl"){
          script{
            env.CARTS_IP = "${sh(script:'kubectl get svc carts -n $CARTS_ENV -o jsonpath=\'{.status.loadBalancer.ingress[0].ip}\'', returnStdout: true)}"
          }
        }
        container("curl") {
          sh "echo ${env.CARTS_IP}"
          sh 'while true; do curl -X POST -H "Content-Type: application/json" -d \"{"id":"3395a43e-2d88-40de-b95f-e00e1502085b", "itemId":"03fef6ac-1896-4ce8-bd69-b798f85c6e0b"}\" http://${env.CARTS_IP}/carts/1/items && sleep 0.1; done'
        }
      }
    }
  }
}


// while true; do curl -X POST -H "Content-Type: application/json" -d "{\"id\":\"3395a43e-2d88-40de-b95f-e00e1502085b\", \"itemId\":\"03fef6ac-1896-4ce8-bd69-b798f85c6e0b\"}" http://${env.CARTS_IP}/carts/1/items && sleep 0.1; done