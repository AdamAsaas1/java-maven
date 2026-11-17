pipeline {
    agent {
        docker {
            image 'my-maven-git:latest'
            args '-v maven-repo:/root/.m2'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                deleteDir()
                checkout scm
            }
        }
        stage('Debug Git') {
            steps {
                sh 'pwd'
                sh 'ls -la'
                sh 'git status || echo "NO GIT REPO"'
            }
        }
        stage('Build & Test') {
            steps {
                dir('maven') {
                    sh 'mvn clean test package'
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
        stage('Run') {
            steps {
                dir('maven') {
                    sh 'java -jar target/maven-1.1.1-SNAPSHOT.jar'
                }
            }
        }
    }
}
