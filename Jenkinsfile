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

                    // Decode the input_csv parameter
                    def decodedContent = new String(Base64.decoder.decode(params.input_csv), "UTF-8")

                    writeFile(file: 'decoded_input.csv', text: decodedContent)

                    echo "Content of decoded file: ${decodedContent}"

                    // Run the Python script with the decoded input file
                    sh (script: "python ${pythonScript} decoded_input.csv")
                }
            }
        }
    }
}