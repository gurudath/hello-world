properties([pipelineTriggers([githubPush()])])

pipeline {
//  agent any
    agent {label 'master'}
    options { disableConcurrentBuilds()
		    timeout(time: 1, unit: 'HOURS')
		    disableResume() 
    }
    triggers {
        cron('H */4 * * 1-5')
    }
    parameters {
        string(name: 'STRING_VARIABLE', defaultValue: 'TestTrainer', description: 'Who should I say hello to?')
        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')
        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    environment {
        def name = "my name"
        def pwdv = ""
    }
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Validate') {
            when { branch 'master' }	
            steps {
                script {
                    if("${STRING_VARIABLE}" == "TestTrainer") {
                        error 'Triggered error'
                    } else {
                        echo "The input was different from TestTrainer so proceeding"
                    }
                }
            }
        }
        stage('GIt Clone Tag and Branch') {
            environment { 
                SERVICE_CREDS = credentials('TRAINER')
            }
            steps {
                sh 'echo "Service user is $SERVICE_CREDS_USR"'
                checkout([$class: 'GitSCM', branches: [[name: 'release-1.0']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Arvind9719/hello-world.git']]])    
            }
        }
        stage('GIt Clone Branch') {
            steps {
                git branch: 'main', url: 'https://github.com/Arvind9719/hello-world.git'
                dir('MOVE') {
                    sh ("touch test.txt")
                    sh ("cat test")
                    readFile 'test'
                }
            }
        }    
        stage('Gmkdir') {
            steps {
                sh "mkdir -p abc/ghi/xyz"
                // timeout(time: 10, unit: 'SECONDS') {
                //     sleep 20
                // }
            }
        }             
        stage('Hello') {
            steps {
                dir('abc') {
                    dir('ghi') {
                        dir('xyz') {
                            sh("touch fffffff.txt")
                        }
                    }
                }
                dir('abc/ghi/xyz') {
                   sh("touch ttttttttttt.txt")
                }
                echo 'Hello World'
                dir('abc/ghi') {
                   deleteDir()
                }
                timestamps {
                    echo "asdasd"
                }
            }
        }
        stage('Trainer') {
            steps {
                script {
                    sh("echo ${STRING_VARIABLE}")
                }
            }
        }  
        stage('Variable Initialization') {
            steps {
                script {
                    sh("echo ${name}")
                    writeFile file: 'test-write-file', text: 'asdasdasdasdasd'
                }
            }
        }
        stage('Using Credencials') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'TRAINER', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                        sh("echo ${USERNAME}")
                    }
                    withCredentials([string(credentialsId: '1111', variable: 'TEXT')]) {
                       sh("echo ${TEXT}")
                    }
                }
            }
        }
        stage('Confirm button') {
            steps {
                //input 'Please confirm'
                sh("echo asdasdasd")
            }
        }
        stage('Child Jobs') {
            steps {
                build quietPeriod: 6, wait: true, job: 'devops-1'
                /*build quietPeriod: 6, wait: true, job: 'devops-2'
                build quietPeriod: 6, wait: true, job: 'tester-1'
                build quietPeriod: 6, wait: true, job: 'tester-2'
                build quietPeriod: 6, wait: true, job: 'developer-2'*/
            }
        }
        stage('Loop') {
            steps {
                echo 'Hello World'

                script {
                    def browsers = ['chrome', 'firefox']
                    for (int i = 0; i < browsers.size(); ++i) {
                        echo "Testing the ${browsers[i]} browser"
                    }
                }
            }
        }
        stage('Parallel Stage') {
            // when { branch 'master' }
            failFast true
            parallel {
                stage('Branch A') {
                    agent {
                        label "master"
                    }
                    steps {
                        echo "On Branch A"
                    }
                }
                stage('Branch B') {
                    agent {
                        label "master"
                    }
                    steps {
                        echo "On Branch B"
                    }
                }
		}
	  }
    }
    post { 
        always { 
            echo 'Run the steps in the post section regardless of the completion status of the Pipeline’s or stage’s run.'
        }
        failure {
            echo 'Only run the steps in post if the current Pipeline’s or stage’s run is successful and the previous run failed or was unstable.'
        }
        aborted {
            echo 'Only run the steps in post if the current Pipeline’s or stage’s run has an "aborted" status, usually due to the Pipeline being manually aborted. This is typically denoted by gray in the web UI.'
        }
        success {
            echo 'Only run the steps in post if the current Pipeline’s or stage’s run has a "success" status, typically denoted by blue or green in the web UI.'
        }
        unstable {
            echo 'Only run the steps in post if the current Pipeline’s or stage’s run has an "unstable" status, usually caused by test failures, code violations, etc. This is typically denoted by yellow in the web UI.'
        }
        unsuccessful {
            echo 'Only run the steps in post if the current Pipeline’s or stage’s run has not a "success" status. This is typically denoted in the web UI depending on the status previously mentioned.'
        }
        cleanup {
            echo 'Run the steps in this post condition after every other post condition has been evaluated, regardless of the Pipeline or stage’s status.'
        }
      }
}

