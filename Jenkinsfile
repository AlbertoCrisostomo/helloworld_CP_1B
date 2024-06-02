pipeline {
    agent any

    stages {
        stage('Get Code') {
            steps {
                echo 'Inicio de la clonación del código fuente!!!'
                git 'https://github.com/AlbertoCrisostomo/helloworld_CP_1B.git'
            }
        }

        stage('Coverage') {
            steps {
                echo 'Inicio de las pruebas Coverage!!!'
                bat '''
                    set PYTHONPATH=%WORKSPACE%
                    coverage run --branch --source=app --omit=app\\__init__.py,app\\api.py -m pytest --junitxml=result-unit.xml test\\unit
                    coverage xml
                '''
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                    cobertura coberturaReportFile: 'coverage.xml', onlyStable: false, failUnstable: false, conditionalCoverageTargets: '100,80,90', lineCoverageTargets: '100,85,95'
                }
            }
        }
        
        stage('Unit') {
            steps {
                echo 'Inicio de las pruebas Unit!!!'             
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                    bat '''
                        set PATH=C:\\Python312;C:\\Python312\\Scripts;
                        set PYTHONPATH=%WORKSPACE%
                        pytest --junitxml=result-unit.xml test\\unit
                    '''
                    junit 'result-unit.xml'
                }
            }
        }
        


        stage('Static') {
            steps {
                echo 'Inicio de las pruebas Static!!!'
                bat '''
                    flake8 --exit-zero --format=pylint app >flake8.out
                '''
                recordIssues tools: [flake8(name: 'Flake8', pattern: 'flake8.out')], qualityGates: [[threshold: 8, type: 'TOTAL', unstable: true], [threshold: 10, type: 'TOTAL', unstable: false]]
            }
        }

        stage('Security') {
            steps {
                echo 'Inicio de las pruebas Security!!!'
                bat '''
                    bandit --exit-zero -r . -f custom -o bandit.out --severity-level medium --msg-template "{abspath}:{line}: [{test_id}] {msg}"
                '''
                recordIssues tools: [pyLint(name: 'Bandit', pattern: 'bandit.out')], qualityGates: [[threshold: 2, type: 'TOTAL', unstable: true], [threshold: 4, type: 'TOTAL', unstable: false]]
            }
        }
           
        stage('Performance') {
            steps {
                echo 'Inicio de las pruebas Performance!!!'
                bat '''
                    C:\\Users\\soporte_bcrp\\Documents\\apache-jmeter-5.6.3\\bin\\jmeter -n -t test\\jmeter\\TestPlan01.jmx -f -l TestPlan01.jtl
                '''
                perfReport sourceDataFiles: 'TestPlan01.jtl'
            }
        }
        
    }
}
