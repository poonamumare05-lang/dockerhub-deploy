
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
                    echo "===== JAVA HOME ====="
                    echo "$JAVA_HOME"

                    echo "===== JAVA VERSION ====="
                    java -version

                    echo "===== JAVAC VERSION ====="
                    javac -version

                    echo "===== MAVEN VERSION ====="
                    mvn -version

                    echo "===== COMPILER LOCATION ====="
                    which javac
                '''
            }
        }

        stage('Build using Maven') {
            steps {
                sh '''
                    export JAVA_HOME="$JAVA_HOME"
                    export PATH="$JAVA_HOME/bin:$PATH"

                    echo "Using compiler:"
                    "$JAVA_HOME/bin/javac" -version

                    mvn clean package -DskipTests
                '''
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
                    docker run -d --name myappcontainer -p 8082:8080 myapp:latest
                '''
            }
        }
    }
}

    
