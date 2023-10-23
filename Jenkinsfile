pipeline {
    agent any
    triggers { cron('0 23 * * SAT') }
    stages {
        stage('vcs') {
            steps {
                git branch: 'qa', url: 'https://github.com/AnasAnsar1/StudentCoursesRestAPI.git'
                stash: 'SCRA'
            }
        }
        stage('Image_build_&_push') {
            agent { label 'Docker' }
            steps {
                unstash 'SCRA'
                sh "docker image build -t anasansarii/SCRA:$env.BRANCH_NAME-$env.BUILD_ID"
                sh "docker image push anasansarii/SCRA:$env.BRANCH_NAME-$env.BUILD_ID"
            }
        }
        stage('deployment') {
            agent { label 'k8s' }
            steps {
                unstash 'SCRA'
                sh "cd deployments/overlays/$env.BRANCH_NAME/ && kustomize edit set image anasansarii/SCRA=anasansarii/SCRA:$env.BRANCH_NAME-$env.BUILD_ID"
                sh "kubectl apply -k deployments/overlays/$env.BRANCH_NAME/"
            }
        }
    }
}