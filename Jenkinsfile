def JOB_PY = "https://github.com/khoatran1602/py-script.git"

pipeline {
    agent any

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
                    def decodedContent = sh(script: "echo '${params.csvFile}' | base64 --decode", returnStdout: true).trim()
                    
                    echo "Decoded CSV file content:"
                    echo decodedContent
                        
                    writeFile(file: 'tempFile.csv', text: decodedContent)

                    // Run the Python script with the decoded input file
                    sh "python ${pythonScript} tempFile.csv"
                }
            }
        }
    }
}