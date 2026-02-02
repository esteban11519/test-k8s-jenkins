pipeline {
  agent any

  environment {
    KUBECONFIG_CRED = 'microk8s-kubeconfig'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Deploy to MicroK8s') {
      steps {
        withCredentials([file(credentialsId: env.KUBECONFIG_CRED, variable: 'KUBECONFIG')]) {
          sh(
            label: 'Deploy to MicroK8s',
            script: '''#!/usr/bin/env bash
set -euo pipefail

echo "Using kubeconfig at: $KUBECONFIG"

# Option A: use kubectl if installed
kubectl version --client=true
kubectl apply -f k8s/deployment.yaml
kubectl -n demo rollout status deploy/hello-nginx --timeout=120s

kubectl -n demo get deploy,svc,pods -o wide
'''.stripIndent()
          )
        }
      }
    }
  }
}
