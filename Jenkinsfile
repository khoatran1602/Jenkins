def JOB_PY = "https://github.com/khoatran1602/py-script.git"

pipeline {
    agent any

    parameters {
        file(name: 'input.csv', description: 'CSV file to upload')
    }

    stages {
        stage('Upload a CSV') {
            steps {
                script {
                    //print the file name
                    echo "${params.input.csv}"
                }
            }
        }
        stage('Run Validation Script') {
            steps {
                git branch: 'main', url: JOB_PY
                script {
                    def pythonScript = 'main.py'
                    sh (script: "python3 ${pythonScript} input.csv", returnStatus: true)

                    if (fileExists('duplicates.txt')) {
                        def duplicates = readFile 'duplicates.txt'
                        error("Duplicate rows found:\n${duplicates}")
                    }
                }
            }
        }
    }
}