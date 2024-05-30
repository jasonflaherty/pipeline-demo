pipeline {
    agent { docker { image 'python:3.12.1-alpine3.19' } }
    stages {
        stage('Initialize'){
            def dockerHome = tool 'myDocker'
            env.PATH = "${dockerHome}/bin:${env.PATH}"
        }
        stage ('Print') {
            steps {
                echo "Hello Noodles!"
            }
        }
        stage('Download Feed') {
            steps {
                sh 'wget http://localhost:8080/rssAll -O feed.xml'
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
