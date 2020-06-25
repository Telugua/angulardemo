pipeline{
  agent any
  stages{
    stage ('checkout'){
      steps{
        sh 'git checkout master'
      }
    }
   stage ('Build'){
       parallel {
             stage("Angular Build") {
                  agent {
                      docker { image 'node:10' }
                  }
                  steps {
                         sh '''
                               echo Installing packages
                               npm install
                               npm install -g @angular/cli@8
                               echo Building Angular Project
                               ng build
                         '''
                  }
             }
              
       }
   }
    stage ('push') {
      steps{
    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'deploy_task', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
        sh 'aws s3 ls'
        sh 'aws s3 sync . s3://sample-angular-demo/ --region us-east-2'
        }
      }
    }
    
    }
}
