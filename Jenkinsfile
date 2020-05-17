pipeline{
    agent any
    stages{
        stage('Lint HTML') {
              steps {
                  echo "11" 
              }
         }
        stage('Upload to AWS') {
              steps {
                  retry(3){         
                    withAWS(region:'us-east-2',credentials:'aws-static') {
                    sh 'echo "Uploading content with AWS creds"'
                        s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'s3-static-jenkis')
                    }
                  }
              }
         }
         stage('Check if site is up') {
              steps {
                  retry(3){
                      
                      sh 'curl -X GET "http://s3-static-jenkis.s3-website.us-east-2.amazonaws.com/index.html"'
                  }
              }
         }
    }
}
