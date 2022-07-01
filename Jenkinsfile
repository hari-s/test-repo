node {
  def commit_id
  stage('Preparation') {
    checkout scm
    sh 'git rev-parse --short HEAD > .git/commit-id'
    commit_id = readFile('.git/commit-id').trim()
  }

  stage('Test') {
    node(nodeJsInstallationName: 'nodejs') {
      sh 'npm install --only-dev'
      sh 'npm test'
    }
  }

  stage('Docker build/publish') {
    docker.withRegistery('https://index.docker.io/v1/', 'dockerhub') {
      def app = docker.build('hdss1991/node-test:${commit_id}', '.').push()
    }
  }
}