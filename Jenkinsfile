pipeline {
    agent any

    stages {
        stage('Sonar Analysis') {
            steps {
                echo 'CODE QUALITY CHECK'
                // Below command works in jenkins 
                sh 'cd webapp'
                sh 'sudo docker run --rm -e SONAR_HOST_URL="http://172.191.99.210:9000" -v ".:/usr/src" -e SONAR_TOKEN="sqp_8bb73d983af3b9c2893c677dd5467879bbc628c2" sonarsource/sonar-scanner-cli -Dsonar.projectKey=test-sonar'
                echo 'sonar test completed'    
            }
        }

        stage('Build LMS') {
            steps {
                echo 'Build LMS starting'
                sh 'cd webapp && npm install && npm run build'
                echo 'Build Completed'
            }
        }

        stage('Release LMS') {
            steps {
                script {
                    //def packageJson = readJSON file: 'webapp/package.json'
                    //def packageJSONVersion = packageJson.version
                    //echo "${packageJSONVersion}"
                    //sh "zip webapp/lms-${packageJSONVersion}.zip -r webapp/dist"
                    //sh "curl -v -u admin:lms12345 --upload-file webapp/lms-${packageJSONVersion}.zip http://172.212.227.13:8081/repository/lms/"
                    sh "zip lms-by-cicd-V1.zip -r dist/*"
                    sh "curl -v -u admin:123 --upload-file lms-front-1.1.zip http://52.149.182.114:8081/repository/lms-front/"
                }
            }
        }

        stage('Deploy LMS') {
            steps {
                script {
                    //def packageJson = readJSON file: 'webapp/package.json'
                    //def packageJSONVersion = packageJson.version
                    //echo "${packageJSONVersion}"
                    //sh "curl -u admin:lms12345 -X GET \'http://172.212.227.13:8081/repository/lms/lms-${packageJSONVersion}.zip\' --output lms-'${packageJSONVersion}'.zip"
                    //sh 'sudo rm -rf /var/www/html/*'
                    //sh "sudo unzip -o lms-'${packageJSONVersion}'.zip"
                    //sh "sudo cp -r webapp/dist/* /var/www/html"
                    sh "curl -u admin:123 -X GET 'http://52.149.182.114:8081/repository/lms-front/lms-front-1.1.zip' --output lms-front-1.1.zip"
                    sh "unzip -o lms-front-1.1.zip"
                    sh "sudo cp -r dist/* /var/www/html"
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