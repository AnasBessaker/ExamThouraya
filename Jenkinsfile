pipeline {
    agent {label 'nexus'}
    environment{ 
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
    }

    stages {
        stage('Hello') {
            steps {
                sh 'mvn --version'
                sh 'ls /home/vagrant'
            }
        }
        stage ('GIT') {
            steps {
               echo "Getting Project from Git"; 
                git branch: "main", 
                    url: "https://github.com/AnasBessaker/ExamThouraya.git";
            }
        }
          //stage('Unit Testing : Test Dynamique Junit and  Mockito'){
            //steps {
           //     sh "mvn clean test -Ptest";
         //   }
       // }

        stage("Build Artifact") {
            steps {
                sh "mvn clean install package";
            }
        }



        stage("Sonar") {
            steps {
                sh 'mvn sonar:sonar -Dsonar.login="admin" -Dsonar.password="sonarqube" '
            }
        }
        stage("compile") {
            steps {
                sh "mvn compile";
            }
        }

        stage("Build artifact") {
            steps {
                sh "sudo docker build -t anasbessaker/exam .";
            }
        }
        stage('Deploy Artifact to Nexus') {
            steps {
                sh 'mvn deploy -Dmaven.test.skip=true'
            }
        }
        stage('Deploy Image to DockerHub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin';
                sh 'sudo docker push anasbessaker/exam';
            }
        }


        stage("docker compose") {
            steps {
                sh "sudo docker compose up -d";
            }
        }
    }



}