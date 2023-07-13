def JOB_PY = "https://github.com/khoatran1602/py-script.git"

pipeline {
    agent any

    parameters {
        base64File(name: 'input_csv', description: 'CSV file to upload')
    }

    stages {
        stage('Run Validation Script') {
            steps {
                git branch: 'main', url: JOB_PY
                script {
                    def pythonScript = 'main.py'

                    // Decode the base64-encoded file content
                    sh (script: "echo \$input_csv | base64 -d > decoded_input.csv", returnStatus: true)

                    writeFile(file: 'tempFile.csv', text: decodedContent)

                    echo "Content of decoded file: ${decodedContent}"

                    // Run the Python script with the decoded input file
                    sh (script: "python3 ${pythonScript} decoded_input.csv", returnStatus: true)

                    if (fileExists('duplicates.txt')) {
                        def duplicates = readFile 'duplicates.txt'
                        error("Duplicate rows found:\n${duplicates}")
                    }
                }
            }
        }
    }
}