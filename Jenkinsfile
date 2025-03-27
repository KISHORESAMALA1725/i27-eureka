pipeline {
    agent any {
        stages {
            stage ('Method Example') {
                steps {
                    script {
                        sh "text(siva).call()"
                    }
                }
            }
        }
    }
}

def text(name) {
    return {
        echo "Hello Mr $name"
    }
}
