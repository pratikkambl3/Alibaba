pipeline {
     agent any
     parameters {
  choice choices: ['DEV', 'QA', 'UAT'], name: 'ENV'
               }

     triggers {
  pollSCM '* * * * *'
           }

     stages{
             stage("Checkout Scm") {
                    steps{
                           checkout scm
                          }
                                    }
             stage("Build"){
                    steps{
                    sh '/home/vboxuser/Documents/DevopsTools/apache-maven-3.9.6/bin/mvn install'
                          }
                               }
              stage("Failed-Notification"){
                      steps{
                                 slackSend baseUrl: 'https://hooks.slack.com/services/', channel: '#alibaba', color: 'warning', failOnError: true, message: 'Build is Failed', teamDomain: 'DEVOPS', tokenCredentialId: 'Alibaba', username: 'alexa'
                            }
                                   }
              stage("Deployment"){
                      steps{
                          sh '''if [ $ENV = "DEV" ];then
cp /target/Alibaba.war /home/vboxuser/Documents/DevopsTools/apache-tomcat-9.0.88/webapps/
echo "Deployment done to DEV SERVER"
elif [ $ENV = "QA"];then
cp /target/Alibaba.war /home/vboxuser/Documents/DevopsTools/apache-tomcat-9.0.88/webapps/
echo "Deployment done to QA server"
elif [ $ENV = "UAT" ];then
cp /target/Alibaba.war /home/vboxuser/Documents/DevopsTools/apache-tomcat-9.0.88/webapps/
echo "Deployment done to UAT server"
fi
'''
  }

                                  }
            stage("Notification"){
                      steps{
                                 slackSend baseUrl: 'https://hooks.slack.com/services/', channel: '#alibaba', color: 'good', failOnError: false, message: 'Build is success', teamDomain: 'DEVOPS', tokenCredentialId: 'Alibaba', username: 'alexa'
                            } 
                                   }
      }

}
