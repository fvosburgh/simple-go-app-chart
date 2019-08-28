podTemplate(yaml: """
kind: Pod
metadata:
  name: helm-slave
spec:
  containers:
  - name: helm-slave
    image: poc-dtr-1.boxboat.net/admin/demo/helm-slave
    imagePullPolicy: Always
    tty: true
    volumeMounts:
      - name: jenkins-kube-cfg
        mountPath: /root
  volumes:
  - name: jenkins-kube-cfg
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
        sh 'helm install --kubeconfig /root/.kube/config --kube-context training-context -n simple-go-app --namespace simple-go-app ./simple-go-app/'
      }
    }
  }
}
