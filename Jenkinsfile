pipeline {
    agent { label 'agent1' }  // Run entire pipeline on the agent with this label

    environment {
        NOTIFY_EMAIL = 'G.Devipriya@Xoriant.Com' // Email for notifications
    }

    #triggers {
        #pollSCM('H/5 * * * *') // Poll the GitHub repo every 5 minutes for changes
    #}

    stages {

        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                git branch: 'main', url: 'https://github.com/g-devipriya-xor/jenkins-ci-test.git'
            }
        }

        stage('Lint / Validation') {
            steps {
                echo "Running basic lint/validation..."
                sh '''
                # Check if ESLint (JS) is installed
                if command -v eslint >/dev/null 2>&1; then
                    eslint . || true
                # Check if Flake8 (Python) is installed
                elif command -v flake8 >/dev/null 2>&1; then
                    flake8 . || true
                else
                    echo "No linter installed, skipping lint step"
                fi
                '''
            }
        }

        stage('Build / Package') {
            steps {
                echo "Building / packaging artifacts..."
                sh 'mkdir -p build && echo "Hello World" > build/sample.txt'
            }
        }

        stage('Archive Artifacts') {
            steps {
                echo "Archiving artifacts..."
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded! Sending success notification...'
            mail to: "${NOTIFY_EMAIL}",
                 subject: "Jenkins Build Success: ${currentBuild.fullDisplayName}",
                 body: "Good news! The build succeeded.\n\nCheck the details here: ${env.BUILD_URL}"
        }
        failure {
            echo 'Pipeline failed! Sending failure notification...'
            mail to: "${NOTIFY_EMAIL}",
                 subject: "Jenkins Build Failed: ${currentBuild.fullDisplayName}",
                 body: "Alert! The build failed.\n\nCheck the details here: ${env.BUILD_URL}"
        }
    }
}
