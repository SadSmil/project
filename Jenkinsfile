pipeline {
    agent {
        docker { 
            image 'postman/newman:latest'
            args '-u=root --entrypoint='
        }  
    }
    
    parameters {
        choice(name: 'Environements', choices: ['Env_1', 'Env2', 'Env3'], description: 'Choix des environnements')
        booleanParam(name: 'select_tout', defaultValue: false, description: 'Lancer tout')
        booleanParam(name: 'selection', defaultValue: false, description: 'Lancer selon le choix')
        booleanParam(name: 'cocher_collection', defaultValue: false, description: 'Cocher pour inclure la collection dans le run')
    }

    stages {
        stage('Lancer le test') {
            steps {
                script {
                    if (params.select_tout) {
                        echo "Option 'Lancer tout' activée. Exécution générale..."
                        sh 'newman run collection.json'
                        sh "newman run env1_collection.json -e environements/env1.postman_environment.json"
                        sh "newman run env2_collection.json -e environements/env2.postman_environment.json"
                    } 
                
                    else if (params.selection) {
                        echo "Option 'Lancer selon le choix' activée."
                        
                        // On vérifie si la case de la collection a bien été cochée
                        if (params.cocher_collection) {
                            echo "La collection est cochée. Application de l'environnement : ${params.Environements}"
                            
                            // Switch classique pour injecter le bon fichier selon l'environnement choisi
                            switch(params.Environements) {
                                case 'Env_1':
                                    sh "newman run env1_collection.json -e environements/env1.postman_environment.json"
                                    break
                                case 'Env2':
                                    sh "newman run env2_collection.json -e environements/env2.postman_environment.json"
                                    break
                                case 'Env3':
                                    sh "newman run env3_collection.json -e environements/env3.postman_environment.json"
                                    break
                                default:
                                    error "Environnement non supporté : ${params.Environements}"
                                    break
                            }
                        } else {
                            echo "L'option 'Lancer selon le choix' est cochée, mais la case 'cocher_collection' ne l'est pas. Rien à lancer."
                        }
                    } 
                    
                    // 3. Si aucune des cases principales n'est cochée
                    else {
                        echo "Aucun mode d'exécution sélectionné (ni 'Lancer tout', ni 'Lancer selon le choix')."
                    }
                }
            }
        }
    }
}