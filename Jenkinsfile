pipeline {
    agent any
    stages {
        stage ('Print') {
            steps {
                echo "Hello Noodles!"
            }
        }
        stage('Download Feed') {
            steps {
                script{
                    if('wget --version' != null) {
                        sh 'wget --version'
                        sh 'wget http://localhost:8080/rssAll -O feed.xml'    
                    } else {
                        echo 'No WGET here...'
                    }
                }
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
