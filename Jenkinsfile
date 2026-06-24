pipeline {
    agent {
        docker { 
            image 'postman/newman:latest'
            args '-u=root --entrypoint='
        }  
    }
    
    parameters {
        booleanParam(name: 'LANCER_TOUS_LES_TESTS', defaultValue: false, description: 'Cocher pour lancer TOUTES les collections')
        choice(name: 'COLLECTION_CHOICE', choices: ['collection', 'env1_collection', 'env2_collection'], description: 'Choisissez la collection à lancer')
        choice(name: 'ENV_CHOICE', choices: ['AUCUN', 'ENV1', 'ENV2'], description: 'Choisissez environnement')
    }

    stages {
        stage('Lancer le test') {
            steps {
                script {
                    if (params.LANCER_TOUS_LES_TESTS) {
                        echo "Lancement global de toutes les collections"
                        sh 'newman run collection.json'
                        sh "newman run env1_collection.json -e environements/env1.postman_environment.json"
                        sh "newman run env2_collection.json -e environements/env2.postman_environment.json"
                    } 
                    else {                
                        def envFlag = ""
                        if (params.ENV_CHOICE == 'ENV1') {
                            envFlag = "-e environements/env1.postman_environment.json"
                        } else if (params.ENV_CHOICE == 'ENV2') {
                            envFlag = "-e environements/env2.postman_environment.json"
                        }
                        // Si params.ENV_CHOICE == 'AUCUN', envFlag reste vide ""
                        sh "newman run ${params.COLLECTION_CHOICE}.json ${envFlag}"
                    }
                }
            }
        }
    }
}