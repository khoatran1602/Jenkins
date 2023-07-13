// def JOB_PY = "https://github.com/khoatran1602/py-script.git"

// pipeline {
//     agent any

//     parameters {
//         base64File(name: 'input_csv', description: 'CSV file to upload')
//     }

//     stages {
//         stage('Run Validation Script') {
//             steps {
//                 git branch: 'master', url: JOB_PY
//                 script {
//                     def pythonScript = 'main.py'

//                     // Decode the input_csv parameter
//                     def decodedContent = new String(Base64.decoder.decode(params.input_csv), "UTF-8")

//                     writeFile(file: 'decoded_input.csv', text: decodedContent)

//                     echo "Content of decoded file: ${decodedContent}"

//                     // Run the Python script with the decoded input file
//                     sh (script: "python ${pythonScript} decoded_input.csv")
//                 }
//             }
//         }
//     }
// }

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
                        echo "Content of tempFile.csv: ${readFile('tempFile.csv')}"
                    } else {
                        echo "No CSV file provided. Skipping upload."
                    }
                }
            }
        }
    }
}