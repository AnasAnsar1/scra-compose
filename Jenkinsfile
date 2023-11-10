pipeline {
  agent { label "gol" }

  triggers { pollSCM('* * * * *') }

  stages {
    stage ("scm") {
      steps {
        git branch: "dev", url: "https://github.com/AnasAnsar1/scra-compose.git"
      }
    }

    stage ("build & push") {
      steps {
        sh "docker image build -t anasansarii/sc:1.1 ."
        sh "docker push anasansarii/sc:1.1"
    }
    }
  }

}
