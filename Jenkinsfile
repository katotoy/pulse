def TARGET_LBU=""

pipeline {
  agent any

  stages {
    stage('Pull Code') {
      steps {

        script {
          properties([
            parameters([
              choice(choices: ['MAIN', 'PH'],  name: 'LBU')
            ])
          ])     
        }

        deleteDir()
          // sh "rm -vrf ${CONFIG_DIR}"
        git branch: 'main', changelog: false, credentialsId: '23030b55-4637-4961-ab94-13a5a10e10f4', poll: false, url: 'https://github.com/katotoy/pulse'
      }
    }

    stage('Copying config files'){
      steps {
        script {

          def remote = [:]
          remote.name = "targetHost"
          remote.host = "192.168.0.15"
          remote.allowAnyHosts = true

          // TARGET_LBU = sh(script: "echo ${params.LBU}", , returnStdout: true).trim()          
          // echo $TARGET_LBU 
          echo "${params.LBU}"

          echo "remotehost: ${remote.host}"
          withCredentials([sshUserPrivateKey(credentialsId: '76521a13-a26d-485e-90c9-5ec5ad0f3d67', keyFileVariable: 'identifyFile', passphraseVariable: '', usernameVariable: 'userName')]) {
            remote.user = userName
            remote.identityFile = identifyFile
            stage("Copying files to server") {
              sshCommand remote: remote, command: "rm -vrf /opt/tomcat/latest/webapps/pulse/main"
              sshPut remote: remote, from: '/var/jenkins_home/workspace/pulse_deploy/main', into: '/opt/tomcat/latest/webapps/pulse'
              sshCommand remote: remote, command: "systemctl restart tomcat8"
                //sshRemove remote: remote, path: CONFIG_DIR
                //sshPut remote: remote, from: CONFIG_DIR, into: '/tmp/'
            }
          }
        }
      }
    }

  }
}