podTemplate(yaml: """
kind: Pod
metadata:
  name: helm-slave
spec:
  imagePullSecrets:
  - name: harbor-creds
  containers:
  - name: jnlp
    image: harbor.training.boxboat.io/library/helm-slave
    imagePullPolicy: Always
    command: ["/bin/sh"]
    tty: true
    volumeMounts:
      - name: jenkins-kube-cfg
        mountPath: /root/.kube/config
        subpath: config
        readOnly: true
  volumes:
  - name: jenkins-kube-cfg
    secret:
      secretName: jenkins-kubeconfig
"""
  ) {
  node(POD_LABEL) {
    stage('checkout') {
      checkout scm
    }
    stage('deploy') {
      container(name: 'jnlp', shell: '/bin/sh') {
        sh 'helm install -n simple-go-app --namespace simple-go-app ./simple-go-app'
      }
    }
  }
}
