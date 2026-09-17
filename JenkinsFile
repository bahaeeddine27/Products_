pipeline {
    agent any

      stages{

        stage('global stage'){
            
            agent{
                docker{
                    image 'usebruno/cli:latest'
                    args '-u root --entrypoint='
                }
            }
            
            stages{
                stage('clean allure results'){
                    
                    steps{
                        sh '''
                            echo "Suppression du cache Allure..."
                            rm -rf allure-results
                            mkdir -p allure-results
                            echo "Dossier allure-results nettoyé avec succès"
                        '''
                    }
                }
        
                stage('run tests'){
                    steps {
                        sh 'bru run --env-file environments/preprod.yml --reporter-junit allure-results/Test-results.xml'}
                }
            }
        }
    }


    post {
        always {
            archiveArtifacts artifacts: 'allure-results/*', 
            allowEmptyArchive: true
            allure includeProperties: false,
                   jdk: '',
                   results: [[path: 'allure-results/']]
        }
    }
} 