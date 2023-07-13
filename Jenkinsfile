def JOB_PY = "https://github.com/khoatran1602/py-script.git"

pipeline {
    agent {
        any {
            image 'python:3.9'
            args '-u' // Run the container with unbuffered output
        }
    }

    parameters {
        base64File(name: 'input_csv', description: 'CSV file to upload')
    }

    stages {
        stage('Run Validation Script') {
            steps {
                git branch: 'master', url: JOB_PY
                script {
                    echo "Base64-encoded CSV file content: ${params.csvFile}"

                    def pythonScript = 'main.py'
                    def decodedContent = sh(script: "echo '${params.csvFile}' | base64 --decode", returnStdout: true).trim()
                        
                    writeFile(file: 'tempFile.csv', text: decodedContent)

                    echo tempFile.csv

                    // Run the Python script with the decoded input file
                    sh (script: "python ${pythonScript} tempFile.csv")
                }
            }
        }
    }
}