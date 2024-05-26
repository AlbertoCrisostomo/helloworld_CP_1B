pipeline {
    agent any

    stages {
        stage('Get Code') {
            steps {
                git 'https://github.com/AlbertoCrisostomo/helloworld_CP_1B.git'
            }
        }
        
        stage('Test') {
            parallel {
                stage('Unit') {
                    steps {
                        catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                            bat '''
                                set PATH=C:\\Python312;C:\\Python312\\Scripts;
                                set PYTHONPATH=%WORKSPACE%
                                pytest --junitxml=result-unit.xml test\\unit
                            '''
                        }
                    }
                }
        
                stage('Rest') {
                    steps {
                        catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                            bat '''
                                set FLASK_APP=app\\api.py
                                set PATH=C:\\Python312;C:\\Python312\\Scripts;C:\\Program Files\\Java\\jdk-11;C:\\Program Files\\Java\\jdk-11\\bin;
                    
                                start flask run
                                start java -jar C:\\Users\\soporte_bcrp\\Documents\\unir_tarea\\wiremock-standalone-3.5.3.jar --port 9090 --root-dir test\\wiremock
            
                                set PYTHONPATH=%WORKSPACE%
                                pytest --junitxml=result-rest.xml test\\rest
                            '''
                        }
                    }
                }
            }
        }

        stage('Static') {
            steps {
                echo 'Aquí van las pruebas Static!!!'
            }
        }

        stage('Security') {
            steps {
                echo 'Aquí van las pruebas Security!!!'
            }
        }

        stage('Performance') {
            steps {
                echo 'Aquí van las pruebas Performance!!!'
            }
        }

        stage('Coverage') {
            steps {
                echo 'Aquí van las pruebas Coverage!!!'
            }
        }

    }
}
