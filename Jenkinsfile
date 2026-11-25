
pipeline {
    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/poonamumare05-lang/dockerhub-deploy.git'
            }
        }

        stage('Check Java and Maven') {
            steps {
                sh '''
                    echo "Checking Java version"
                    echo $JAVA_HOME
                    java -version
                    javac -version

                    echo "Checking Maven version"
                    mvn -version
                '''
            }
        }

        stage('Build using Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                    docker stop myappcontainer || true
                    docker rm myappcontainer || true
                    docker run -d --name myappcontainer -p 8081:8080 myapp:latest
                '''
            }
        }
    }
}
