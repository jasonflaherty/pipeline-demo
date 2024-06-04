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
                sh 'wget --version'
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
