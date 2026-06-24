pipeline {
    agent {
        docker { 
            image 'postman/newman:latest'
            args '-u=root --entrypoint='
        }  
    }
    
    parameters {
        booleanParam(name: 'LANCER_TOUS_LES_TESTS', defaultValue: false, description: 'Cocher pour lancer TOUTES les collections d\'un coup')
        
        booleanParam(name: 'AVEC_OU_SANS_ENV', defaultValue: false, description: 'Décoché = collection seule sans env | Coché = utiliser un environnement (switch)')
        
        choice(name: 'Environements', choices: ['Env_1', 'Env2'], description: 'Choix de l\'environnement si la case ci-dessus est activée')
    }

    stages {
        stage('Lancer le test') {
            steps {
                script {
                    if (params.LANCER_TOUS_LES_TESTS) {
                        echo "Exécution globale de TOUS les tests..."
                        sh 'newman run collection.json'
                        sh "newman run env1_collection.json -e environements/env1.postman_environment.json"
                        sh "newman run env2_collection.json -e environements/env2.postman_environment.json"
                        sh "newman run env3_collection.json -e environements/env3.postman_environment.json"
                    } 
                    
                    else {
                        
                        if (!params.AVEC_OU_SANS_ENV) {
                            echo "Exécution SANS environnement..."
                            sh 'newman run collection.json'
                        } 
                        
                        else {
                            echo "Exécution ciblée avec environnement. Choix : ${params.Environements}"
                            
                            switch(params.Environements) {
                                case 'Env_1': 
                                    sh "newman run env1_collection.json -e environements/env1.postman_environment.json"
                                    break
                                    
                                case 'Env2': 
                                    sh "newman run env2_collection.json -e environements/env2.postman_environment.json"
                                    break
                                                                    
                                default:
                                    error "Environnement inconnu : ${params.Environements}"
                                    break
                            }
                        }
                    }
                }
            }
        }
    }
}