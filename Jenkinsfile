pipeline {
    agent any
    stages {
        stage ('Print') {
            steps {
                echo "Hello Noodles!"
            }
        }
        stage('Check WGET'){
            steps{
                sh '''
                    wget --post-data="" http://localhost:8080/rssAll
                    cat feed.xml
                '''
            }
        }
        stage('test JAVA') {
            steps {
                sh 'java --version'
            }
        }
        stage('test python'){
            steps{
                sh 'python --version'
            }
        }
        stage('owaspcheck') {
          steps {
            dependencyCheck additionalArguments: ''' 
                        -o './'
                        -s './'
                        -f 'ALL' 
                        --prettyPrint''', odcInstallation: 'owaspcheck'
            
            dependencyCheckPublisher pattern: 'dependency-check-report.xml'
          }
        }
    }
}
