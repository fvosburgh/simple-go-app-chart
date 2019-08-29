@Library('jenkins-shared-library@develop')
import com.boxboat.jenkins.pipeline.common.vault.*
import com.boxboat.jenkins.pipeline.deploy.kubernetes.*
import com.demo.jenkins.pipeline.deploy.*

properties(demoDeployProps.props())

def deploy = new DemoDeploy(
  app: "simple-go-app",
)

node(label: 'docker') {
  deploy.wrap {

    stage ('Deploy') {
      deploy.deploy()
    }

  }
}
