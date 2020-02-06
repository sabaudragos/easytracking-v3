pipeline {
   agent any
   stages {
      stage('Checkout') {
          steps {
            sh '''#!/bin/bash
                echo 'Removing existing project directory'
                rm -rf easytracking-v3
                echo 'Cloning git project'
                git clone 'https://github.com/Callin/easytracking-v3.git'
            '''
          }
      }
      stage('Build') {
          steps {
            sh '''#!/bin/bash
                echo 'Running mvn clean package'
                cd easytracking-v3
                mvn clean package -DskipTests
            '''
            }
      }
      stage('Deploy') {
          steps {
            sh '''#!/bin/bash
                echo 'Copying new version to temporary directory'
                cp -R easytracking-v3/target/easytracking-v3-0.0.1-SNAPSHOT.jar /home/dragos/apps/easytracking/backend/easytracking-v3-0.0.1-SNAPSHOT.jar
                cd /home/dragos/apps/easytracking/backend
                output=`lsof -t -i:6000`
                echo $output
                if [ ${#output} != 0 ]; then
                    echo "Port 6000 is in use. Terminating the process"
                    kill -9 $output
                else
                    echo "Port 6000 is not in use"
                fi
                echo "Start the new process "
                echo $PWD
                echo ja=`java --version`
                java -jar easytracking-v3-0.0.1-SNAPSHOT.jar &
            '''
        }
      }
    }
}
