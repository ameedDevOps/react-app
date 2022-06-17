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
              sh 'docker image build -t ameedqasimi/my-react-app:1.6 .'
              sh 'docker container run -dit -p 2222:3000 ameedqasimi/my-react-app:1.6'
            }
        }
        stage('Push Docker Image in DockerHub') {
            steps {
            withCredentials([usernamePassword(credentialsId: 'My_Public_Docker', passwordVariable: '1fba95b9-09ac-4453-8146-cdc1c6981c03', usernameVariable: 'ameedqasimi')]) {
                sh 'docker login -u ameedqasimi -p 1fba95b9-09ac-4453-8146-cdc1c6981c03'
                sh 'docker push ameedqasimi/my-react-app:1.6'
            }
        }
        }
        stage('DeploymentOnUAT') {
            //environment {
        //dockerRun = 'docker container run -d -p 2222:3000 --name react-demo1 ameedqasimi/my-react-app:1.5'
   // }
            
            steps {
                //sshagent(['Docker_Root_User']) {
                 //   sh "ssh -o StrictHostKeyChecking=no docker@192.168.0.19 
               // ${dockerRun}"
               //}
             sshagent(['Docker_Demo_Server']) {
                 sh 'ssh -o StrictHostKeyChecking=no docker@192.168.0.19 docker container rm -f '${docker ps -a -q}' '
            }
            sshagent(['Docker_Demo_Server']) {
                sh 'ssh -o StrictHostKeyChecking=no docker@192.168.0.19 docker container run -d -p 2222:3000 --name react-demo1 ameedqasimi/my-react-app:1.6'
            }
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
