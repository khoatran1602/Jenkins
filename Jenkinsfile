#!/usr/bin/env groovy

def JOB_PY = "https://github.com/khoatran1602/py-script.git"

pipeline {
    agent any

    environment {
        PATH = "/path/to/python/bin:$PATH"
    }

    parameters {
        base64File(name: 'csvFile', description: 'CSV file to upload')
    }

    stages {
        stage('Run Validation Script') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: JOB_PY]]])
                script {
                    echo "Base64-encoded CSV file content: ${params.csvFile}"

                    def pythonScript = 'main.py'
                    sh(label: 'Decode CSV file', script: "echo '${params.csvFile}' | base64 --decode > tempFile.csv")
                    
                    echo "Decoded CSV file content:"
                    sh(label: 'Display decoded content', script: 'cat tempFile.csv')

                    // Check if Python is available
                    def pythonAvailable = sh(script: "command -v python || command -v python3", returnStatus: true) == 0

                    if (pythonAvailable) {
                        // Run the Python script with the decoded input file
                        sh(label: 'Run Python script', script: "python ${pythonScript} tempFile.csv")
                    } else {
                        error("Python is not found in the current environment. Please ensure Python is installed and available in the PATH.")
                    }
                }
            }
        }
    }
}