pipeline {
    agent {
        docker { 
            image 'postman/newman:latest'
            args '-u=root --entrypoint='
        }  
    }
    
    parameters {
        // Choix de l'environnement (utilisé uniquement si la case "avec_ou_sans_env" est cochée)
        choice(name: 'Environements', choices: ['Env_1', 'Env2'], description: 'Choix de l\'environnement')
        
        // Les 2 cases à cocher de votre logique
        booleanParam(name: 'avec_ou_sans_env', defaultValue: false, description: 'Cocher pour utiliser un environnement (Décoché = Sans environnement)')
        booleanParam(name: 'cocher_collection', defaultValue: false, description: 'Cocher pour valider et lancer l\'exécution')
    }

    stages {
        stage('Lancer le test') {
            steps {
                script {
                    // 1. On vérifie d'abord si la case globale pour lancer la collection est cochée
                    if (params.cocher_collection) {
                        
                        // 2. CAS SANS ENVIRONNEMENT (La case "avec_ou_sans_env" n'est PAS cochée)
                        if (!params.avec_ou_sans_env) {
                            echo "Exécution de la collection principale SANS environnement..."
                            sh 'newman run collection.json'
                        } 
                        
                        // 3. CAS AVEC ENVIRONNEMENT (La case "avec_ou_sans_env" EST cochée)
                        else {
                            echo "Exécution avec environnement activée. Choix : ${params.Environements}"
                            
                            switch(params.Environements) {
                                case 'Env_1':
                                    echo "Lancement de la collection dédiée à Env_1..."
                                    sh "newman run env1_collection.json -e environements/env1.postman_environment.json"
                                    break
                                    
                                case 'Env2':
                                    echo "Lancement de la collection dédiée à Env2..."
                                    sh "newman run env2_collection.json -e environements/env2.postman_environment.json"
                                    break
                                    
                                default:
                                    error "Environnement inconnu : ${params.Environements}"
                                    break
                            }
                        }
                        
                    } 
                    // Si la case de la collection n'est pas cochée, on ne fait rien
                    else {
                        echo "La case 'cocher_collection' n'est pas activée. Aucun test ne sera lancé."
                    }
                }
            }
        }
    }
}