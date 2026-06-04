pipeline {
    // Yeh line ensure karegi ki job MASTER pe nahi, AGENT pe run ho
    agent { label 'remote-linux-node' } 
    
    stages {
        stage('Verify Remote Connection') {
            steps {
                echo '============================================='
                echo '✅ Job successfully running on REMOTE AGENT!'
                echo '============================================='
                sh '''
                    echo "Hostname: $(hostname)"
                    echo "Whoami: $(whoami)"
                    echo "OS Info: $(cat /etc/os-release | grep PRETTY_NAME)"
                '''
            }
        }
    }
}