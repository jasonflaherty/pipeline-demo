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
                sh 'wget http://localhost:8080/rssAll -O feed.xml'
            }
        }
        stage('Parse and Extract Data') {
            steps {
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
