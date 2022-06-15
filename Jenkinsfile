pipeline {
    agent any

    stages {
        stage('Git Clone') {
            steps {
                git branch: 'main', credentialsId: 'github_node01', url: 'https://github.com/ameedDevOps/react-app.git'
            }
        }
        stage('Run-Tests-Parellel') {
                    steps {
                        parallel(
                            "Smoke-Test": {
                                    echo 'Project is Testing'
                                    sleep 5
                                },
                            "Unit-Test": {
                                    echo 'Unit testing is in progress'
                                    sleep 5
                                },
                            "UI-Test": {
                                    echo 'UI testing is in progress'
                                    sleep 5
                                }
                            )
                        }                  
                    }
          stage('Building Docker Image') {
            steps {
              sh 'docker container rm -f $(docker ps -a -q)'
              sh 'docker image build -t ameedqasimi/my-react-app:1.2 .'
              sh 'docker container run -dit -p 2222:3000 my-react-app:1.2'
            }
        }
        stage('Push Docker Image in DockerHub') {
            steps {
            withCredentials([usernamePassword(credentialsId: 'My_Public_Docker', passwordVariable: 'Ameed@123', usernameVariable: 'ameedqasimi')]) {
                sh 'docker login -u ameedqasimi -p Ameed@123'
                sh 'docker push ameedqasimi/my-react-app:1.2'
            }
        }
        }
        stage('Deploying') {
        steps {
            parallel( 
                "first-job": {
                echo 'Deploying on Dev'
                sleep 5
                },
                "Second-Job": {
                echo 'Deploying on Dev'
                sleep 5
                }
            )
            }
        }
        stage('Deploy on Prod'){
            input {
                message "should we continue?"
            }
                steps {
                echo 'Deploying on Prod'
                sleep 5
            }
        }
    }
}
