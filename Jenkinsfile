pipeline {
    agent any 
        stages {
            stage ('Method Example') {
                steps {
                    script {
                        text('saharssh').call()
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
