#!/usr/bin/env groovy

def JOB_PY = "https://github.com/khoatran1602/py-script.git"

pipeline {
    agent any

    environment {
        PATH = "C:/Users/LENOVO/AppData/Local/Programs/Python/Python39/Scripts"
    }

    parameters {
        base64File(name: 'csvFile', description: 'CSV file to upload')
    }

    stages {
        stage('Run Validation Script') {
            steps {
                git branch: 'master', url: JOB_PY
                script {
                    echo "Base64-encoded CSV file content: ${params.csvFile}"

                    def pythonScript = 'main.py'
                    sh(label: 'Decode CSV file', script: "echo '${params.csvFile}' | base64 --decode > tempFile.csv")
                    
                    echo "Decoded CSV file content:"
                    sh(label: 'Display decoded content', script: 'cat tempFile.csv')
                        
                    // Run the Python script with the decoded input file
                    sh(label: 'Run Python script', script: "python3 ${pythonScript} tempFile.csv")
                }
            }
        }
    }
}