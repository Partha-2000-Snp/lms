pipeline {
    agent { label 'lms-fe' }

    stages {
        stage('Sonar Analysis') {
            steps {
                echo 'CODE QUALITY CHECK'
                // Below command works in jenkins 
                sh 'cd webapp'
                sh 'cd webapp && sudo docker run --rm -e SONAR_HOST_URL="http://172.191.99.210:9000" -v ".:/usr/src" -e SONAR_TOKEN="sqp_8bb73d983af3b9c2893c677dd5467879bbc628c2" sonarsource/sonar-scanner-cli -Dsonar.projectKey=test-sonar'
                echo 'sonar test completed'    
            }
        }

        stage('Build LMS') {
            steps {
                echo 'build lms frontend starting'
                sh 'cd webapp && npm install && npm run build'
                echo 'build done'
            }
        }

        stage('Release LMS') {
            steps {
                script {
                    def packageJson = readJSON file: 'webapp/package.json'
                    def Version = packageJson.version
                    echo "${Version}"
                    sh "zip webapp/lms-${Version}.zip -r webapp/dist && pwd && cd webapp && ls"
                    sh "curl -v -u admin:123 --upload-file webapp/lms-${Version}.zip http://52.149.182.114:8081/repository/lms-front/"
                }
            }
        }

        stage('Deploy LMS') {
            steps {
                script {
                    def packageJson = readJSON file: 'webapp/package.json'
                    def Version = packageJson.version
                    echo "${Version}"
                    sh "curl -u admin:123 -X GET \'http://52.149.182.114:8081/repository/lms-front/lms-${Version}.zip\' --output lms-'${Version}'.zip"
                    sh 'sudo rm -rf /var/www/html/*'
                    sh "sudo unzip -o lms-'${Version}'.zip"
                    sh "sudo cp -r webapp/dist/* /var/www/html"
                }
            }
        }

        stage('Clean Up Workspace') {
            steps {
                    echo 'Cleaning Work Space'
                    // Install Cleanup Workspace plugin to make below command work
                    cleanWs()
            }
        }
        
    }
}