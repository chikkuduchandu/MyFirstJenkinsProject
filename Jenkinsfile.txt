pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout from Git repository
                git 'https://github.com/chikkuduchandu/MyFirstJenkinsProject.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Compiling Java files...'
                bat 'javac Book.java Library.java LibrarySystem.java' // Compile Java files
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Assuming you have JUnit tests in LibrarySystemTest.java
                bat 'javac -cp ".;C:\\Users\\chand\\Documents\\V sem\\CSE\\CSE\\DEVOPS\\project\\junit-4.10.jar" LibrarySystemTest.java' // Compile test files
                bat 'java -cp ".;C:\\Users\\chand\\Documents\\V sem\\CSE\\CSE\\DEVOPS\\project\\junit-4.10.jar" org.junit.runner.JUnitCore LibrarySystemTest' // Run tests
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                bat 'jar cvf LibrarySystem.jar Book.class Library.class LibrarySystem.class' // Create JAR file
                bat 'xcopy /Y LibrarySystem.jar "C:\\Users\\chand\\Documents\\V sem\\CSE\\CSE\\DEVOPS\\project\\"' // Copy JAR to destination
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            // Cleanup actions, if needed
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
