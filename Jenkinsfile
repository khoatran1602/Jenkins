// def JOB_PY = "https://github.com/khoatran1602/py-script.git"

// pipeline {
//     agent any

//     environment {
//         PATH = "C:/Users/LENOVO/AppData/Local/Programs/Python/Python39/:$PATH"
//     }

//     parameters {
//         base64File(name: 'csvFile', description: 'CSV file to upload')
//     }

//     stages {
//         stage('Run Validation Script') {
//             steps {
//                 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: JOB_PY]]])
//                 script {
//                     // echo "Base64-encoded CSV file content: ${params.csvFile}"

//                     def pythonScript = 'main.py'
//                     sh(label: 'Decode CSV file', script: "echo '${params.csvFile}' | base64 --decode > tempFile.csv")
                    
//                     // echo "Decoded CSV file content:"
//                     // sh(label: 'Display decoded content', script: 'cat tempFile.csv')

//                     // Check if the file is in CSV format
//                     def isCSV = sh(script: 'head -n 1 tempFile.csv | grep -qE "^([^,]+,)+[^,]+$"', returnStatus: true) == 0

//                     if (isCSV) {
//                         // Check if Python is available
//                         def pythonAvailable = sh(script: 'command -v python || command -v python3', returnStatus: true) == 0

//                         if (pythonAvailable) {
//                             // Run the Python script with the decoded input file
//                             sh(label: 'Run Python script', script: "python3 ${pythonScript} tempFile.csv")
//                         } else {
//                             error("Python is not found in the current environment. Please ensure Python is installed and available in the PATH.")
//                         }
//                     } else {
//                         error("The provided file is not in CSV format.")
//                     }
//                 }
//             }
//         }
//     }
// }

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
        stage('Validation') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: JOB_PY]]])
                script {
                    def pythonValidationScript = 'validate_csv.py'

                    // Decode CSV file
                    writeFile(file: 'tempFile.csv', text: "echo '${params.csvFile}' | base64 --decode")

                    // Validate the file format using Python
                    def pythonAvailable = sh(script: "command -v python || command -v python3", returnStatus: true) == 0

                    if (pythonAvailable) {
                        sh(label: 'Run Python validation script', script: "python3 ${pythonValidationScript} tempFile.csv > validation_status.txt")
                        
                        // Check if the validation was successful
                        def validationStatus = readFile('validation_status.txt').trim()

                        if (validationStatus == "File format is valid: CSV") {
                            echo "File format is valid."
                        } else {
                            error("The provided file is not in CSV format.")
                        }
                    } else {
                        error("Python is not found in the current environment. Please ensure Python is installed and available in the PATH.")
                    }
                }
            }
        }

        stage('Execution') {
            steps {
                script {
                    sh(label: 'Decode CSV file', script: "echo '${params.csvFile}' | base64 --decode > tempFile.csv")

                    def pythonMainScript = 'main.py'

                    // Check if Python is available
                    def pythonAvailable = sh(script: "command -v python || command -v python3", returnStatus: true) == 0

                    if (pythonAvailable) {
                        sh(label: 'Run Python script', script: "python3 ${pythonMainScript} tempFile.csv")
                    } else {
                        error("Python is not found in the current environment. Please ensure Python is installed and available in the PATH.")
                    }
                }
            }
        }
    }
}

