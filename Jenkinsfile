#!/usr/bin/env groovy

def JOB_PY = "https://github.com/khoatran1602/py-script.git"

pipeline {
    agent any

    environment {
        PATH = "C:/Users/LENOVO/AppData/Local/Programs/Python/Python39/:$PATH"
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
                    bat(label: 'Decode CSV file', script: "echo '${params.csvFile}' | base64 --decode > tempFile.csv")
                    
                    echo "Decoded CSV file content:"
                    bat(label: 'Display decoded content', script: 'cat tempFile.csv')
                        
                    // Run the Python script with the decoded input file
                    bat(label: 'Run Python script', script: "python ${pythonScript} tempFile.csv")
                }
            }
        }
    }
}