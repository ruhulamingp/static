pipeline{
    agent any
    stages{
        stage('Lint HTML') {
              steps {
                  sh 'tidy -q -e *.html'
              }
         }
        
        stage('Upload to AWS') {
              steps {
                  retry(3){         
                    withAWS(region:'us-east-1',credentials:'aws-static') {
                    sh 'echo "Uploading content with AWS creds"'
                        s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'ruhulaminjenkins')
                    }
                  }
              }
         }
         stage('Check if site is up') {
              steps {
                  retry(3){
                      sh 'curl -X GET "https://ruhulaminjenkins.s3-website-us-east-1.amazonaws.com/index.html"'
                  }
              }
         }
    }
}
