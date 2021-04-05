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
  }
}