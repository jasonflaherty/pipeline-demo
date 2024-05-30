pipeline {
    agent { docker { image 'python:3.12.1-alpine3.19' } }
    stages {
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
        stage('Parse and Extract Data') {
            script {
                def xml = readXML file: 'feed.xml'
                def items = []
                xml.each { item ->
                    items << [title: item.title.text(), link: item.link.text()]
                }
            }
        }
        stage('Send Email') {
            steps {
                emailext body: '''
                    Hi Team,

                    Here are the new items in your feed:

                    ${items.collect { "<a href='${it.link}'>${it.title}</a>" }.join('\n')}

                    Thanks,
                    The Jenkins Bot
                ''',
                subject: 'New Items in Your Feed',
                to: 'recipients@example.com'
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
