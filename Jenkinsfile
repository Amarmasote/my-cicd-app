pipeline {
    agent any

    tools {
        // These must be configured under "Manage Jenkins" → "Global Tool Configuration"
        jdk 'jdk17'          // Name of your JDK in Jenkins
        maven 'maven3'       // Name of your Maven installation in Jenkins
    }

   // environment {
        // Optional: Set any global variables here
        // BUILD_ENV = 'dev'
   // }

    options {
        timestamps()          // Add timestamps to console output
        ansiColor('xterm')    // Enable colored logs
        buildDiscarder(logRotator(numToKeepStr: '10'))  // Keep last 10 builds
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📦 Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo '⚙️ Building project using Maven...'
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Archive JAR') {
            steps {
                echo '📁 Archiving JAR file...'
                // Save the jar produced in target directory
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Post-Build Reports') {
            steps {
                echo '🧪 Collecting test reports (if available)...'
                // This will show test results if any exist (even though we skip tests now)
                junit '**/target/surefire-reports/*.xml'
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed. Check console output for details.'
        }
        always {
            echo '🧹 Cleaning up workspace...'
            cleanWs()   // requires Workspace Cleanup plugin
        }
    }
}
