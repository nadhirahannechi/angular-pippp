pipeline { 
   agent { 
       docker { 
           image 'trion/ng-cli-karma:1.2.1' 
       } 
   } 
   environment {
      URL='http://artefact.focus.com.tn:8081/repository/webbuild/dist.tar.gz'
      USER='mavenuser:m@venp@$$word'
      
   }
   stages { 
       stage('Checkout') { 
         agent any 
          steps { 
               deleteDir() 
               checkout scm 
          } 
       } 
   stages { 
       stage('NPM Install') { 
          steps { 
               sh 'npm install' 
          } 
       } 
 
 stage('Test') { 
    steps { 
         script { 
         withEnv(["CHROME_BIN=/usr/bin/chromium-browser"]) { 
            sh 'ng test --progress=false --watch false' 
          } 
       }
        junit '**/test-results.xml' 
      }
    }
 
 stage('Build') { 
    steps { 
            sh 'ng build --prod --aot --sm --progress=false' 
          } 
       }
 stage('Archive') { 
 agent none 
    steps { 
           sh 'tar -cvzf dist.tar.gz --strip-components=1 dist' 
           archive 'dist.tar.gz' 
          } 
       } 

 stage('Nexus Upload Stage') {
 agent none 
    steps { 
       sh 'curl -v -u ${USER} --upload-file dist.tar.gz ${URL}' 
    }
}
 
 stage('Deploy') {
 agent none 
 steps { 
 echo "Deploying..." 
    }
   }
}

   }
