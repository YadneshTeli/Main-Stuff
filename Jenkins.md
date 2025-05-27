Jenkins Pipeline
```bash
pipeline {
    agent any

    environment {
        BUILD_VERSION = "1.0"
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    def workspace = env.WORKSPACE
                    def cloneJSP = "${workspace}\\cloneJSP"

                    // Remove existing if exists
                    if (fileExists(cloneJSP)) {
                        bat "rmdir /S /Q \"${cloneJSP}\""
                        echo "Removed existing Repo Folder."
                    }
                    bat "mkdir cloneJSP"
                    bat "git clone https://github.com/YadneshTeli/Ecllipse-Test.git \"${cloneJSP}\""
                }
            }
        }

        stage('Clean Old Deployment') {
            steps {
                script {
                    def workspace = env.WORKSPACE
                    def jspDemoPath = "${workspace}\\JSPDemo"
                    def warFile = "${workspace}\\JSPDemo.war"

                    // Remove existing JSPDemo folder if it exists
                    if (fileExists(jspDemoPath)) {
                        bat "rmdir /S /Q \"${jspDemoPath}\""
                        echo "Removed existing JSPDemo folder."
                    }

                    // Remove any existing WAR file
                    if (fileExists(warFile)) {
                        bat "del /F /Q \"${warFile}\""
                        echo "Removed existing WAR file."
                    }
                }
            }
        }

        stage('Copy Required Files') {
            steps {
                script {
                    def workspace = env.WORKSPACE
                    def sourceDir = "${workspace}\\cloneJSP\\Ecllipse-Test\\src\\main\\webapp"
                    def destinationDir = "${workspace}\\JSPDemo"

                    // Create JSPDemo folder
                    bat "mkdir \"${destinationDir}\""

                    // Copy .jsp files if they exist
                    if (fileExists("${sourceDir}\\*.jsp")) {
                        bat "xcopy /S /E /Y \"${sourceDir}\\*.jsp\" \"${destinationDir}\\\""
                    } else {
                        echo "No .jsp files found to copy."
                    }

                    // Copy .html files if they exist
                    if (fileExists("${sourceDir}\\*.html")) {
                        bat "xcopy /S /E /Y \"${sourceDir}\\*.html\" \"${destinationDir}\\\""
                    } else {
                        echo "No .html files found to copy."
                    }

                    // Copy WEB-INF folder if it exists
                    if (fileExists("${sourceDir}\\WEB-INF")) {
                        bat "xcopy /S /E /Y \"${sourceDir}\\WEB-INF\" \"${destinationDir}\\WEB-INF\\\""
                    } else {
                        echo "WEB-INF folder not found."
                    }

                    echo "Files copied successfully."

                    // Create WAR file
                    bat "C:\\PROGRA~1\\Java\\jdk-21\\bin\\jar -cvf JSPDemo.war -C \"${destinationDir}\" ."
                }
            }
        }

        stage('Deploy') {
            steps {
                bat 'curl -u admin:admin --upload-file JSPDemo.war "http://localhost:8080/manager/text/deploy?path=/JSPDemo&update=true"'
            }
        }

        stage('Deploy WAR to Tomcat') {
            steps {
                script {
                    def appUrl = "http://localhost:8080/JSPDemo/"

                    // Wait for Tomcat to start (Optional)
                    sleep(time: 10, unit: "SECONDS") // Wait for Tomcat to deploy WAR

                    // Open in Firefox
                    bat "start firefox ${appUrl}"
                }
            }
        }
    }
}
```
