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
                    
                    withFileParameter('input_csv') {
                        sh (script: "python3 ${pythonScript} \$input_csv", returnStatus: true)
                    }

                    if (fileExists('duplicates.txt')) {
                        def duplicates = readFile 'duplicates.txt'
                        error("Duplicate rows found:\n${duplicates}")
                    }
                }
            }
        }
    }
}