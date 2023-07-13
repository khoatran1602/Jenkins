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
                    sh (script: "python ${pythonScript} decoded_input.csv")
                }
            }
        }
    }
}

pipeline {
    agent any

    parameters {
        base64File(name: 'csvFile', description: 'Upload the CSV file')
    }

    stages {
        stage('Upload and display CSV file content') {
            steps {
                script {
                    if (params.csvFile) {
                        echo "Base64-encoded CSV file content: ${params.csvFile}"
                        def decodedContent = sh(script: "echo '${params.csvFile}' | base64 --decode", returnStdout: true).trim()
                        
                        // Print the content of the CSV file
                        echo "Decoded CSV file content:"
                        echo decodedContent
                        
                        writeFile(file: 'tempFile.csv', text: decodedContent)
                    } else {
                        echo "No CSV file provided. Skipping upload."
                    }
                }
            }
        }
    }
}