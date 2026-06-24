pipeline {
    agent {
        docker { 
            image 'postman/newman:latest'
            args '-u=root --entrypoint='
        }  
    }
    
    parameters {
        booleanParam(name: 'LANCER_TOUS_LES_TESTS',defaultValue: false, description: 'Cocher pour lancer TOUTES les collections')
        choice( ame: 'ENV_CHOICE', choices: ['AUCUN', 'ENV1', 'ENV2'],description: 'Choisissez environnement ')
    }

    stages {
        stage('Lancer le test') {
            steps {
                script {
                    if (params.LANCER_TOUS_LES_TESTS) {
                        sh 'newman run collection.json'
                        sh "newman run 'env1_collection.json' -e environements/env1.postman_environment.json"
                        sh "newman run 'env2_collection.json' -e environements/env2.postman_environment.json"
                    } 
                    else {
                        if (params.ENV_CHOICE == 'AUCUN') {
                            sh 'newman run collection.json'
                        } 
                        else {
                            switch(params.ENV_CHOICE) {
                                case 'ENV1': 
                                    echo "Lancement sur ENV1"
                                    sh "newman run 'env1_collection.json' -e environements/env1.postman_environment.json"
                                    break
                                case 'ENV2': 
                                    echo "Lancement sur ENV2"
                                    sh "newman run 'env2_collection.json' -e environements/env2.postman_environment.json"
                                    break
                                default:
                                    error "Environnement inconnu : ${params.ENV_CHOICE}"
                                    break
                            }
                        }
                    }
                }
            }
        }
    }
}