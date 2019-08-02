podTemplate(yaml: """
kind: Pod
metadata:
  name: helm-slave
spec:
  containers:
  - name: helm-slave
    image: harbor.training.boxboat.io/library/helm-slave
    imagePullPolicy: Always
    args: \${computer.jnlpmac} \${computer.name}
    tty: true
    volumeMounts:
      - name: jenkins-docker-cfg
        mountPath: /root
  volumes:
  - name: jenkins-docker-cfg
    projected:
      sources:
      - secret:
          name: jenkins-kubeconfig
          items:
            - key: kubeconfig
              path: .kube/config
"""
  ) {
  node(POD_LABEL) {
    stage('checkout') {
      checkout scm
    }
    stage('deploy') {
      container(name: 'helm-slave', shell: '/bin/sh') {
        sh 'helm install -n simple-go-app --namespace simple-go-app .'
      }
    }
  }
}
