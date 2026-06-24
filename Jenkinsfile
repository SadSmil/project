pipeline {
    agent {
        docker { 
            image 'postman/newman:latest'
            args '-u=root --entrypoint='
        }  
    }
    
    parameters {
        choice(name: 'ENV_CHOICE', choices: ['AUCUN', 'ENV1', 'ENV2'], description: 'Choisissez environnement pour tester'
        )
    }

    stages {
        stage('Lancer le test') {
            steps {
                script {
                    if (params.ENV_CHOICE == 'AUCUN') {
                        echo "Exécution de la collection sans environnement"
                        sh 'newman run collection.json'
                    } 
                    else {
                        switch(params.ENV_CHOICE) {
                            case 'ENV_1':
                                sh "newman run collection_avec_env.json -e environements/env1.postman_environment.json"
                                break
                            case 'ENV_2':
                                sh "newman run collection_avec_env.json -e environements/env2.postman_environment.json"
                                break
                        }
                        
                    }
                }
            }
        }
    }
}