pipeline {
    agent any

    stages {
        stage('Import APIs') {
            steps {
                script {
                    // Definir o caminho do repositório
                    def REPOSITORY_PATH = "/var/lib/jenkins/workspace/${env.JOB_NAME}/swaggers"

                    // Verificar se existem arquivos swagger modificados
                    def modified = sh(script: "git diff --name-only HEAD HEAD~1 | grep swagger", returnStdout: true).trim()

                    if (modified) {
                        echo "Modified swagger files found."

                        // Obter diretórios modificados
                        def changed_dirs = sh(script: "git diff --name-only HEAD HEAD~1 | grep swagger | xargs -n1 dirname | sort | uniq", returnStdout: true).trim().split("\n")

                        for (changed_dir in changed_dirs) {
                            echo "Processing $changed_dir"

                            // Obter o caminho do api-config.json
                            def api_config_path = "${REPOSITORY_PATH}/${changed_dir}/config/api-config.json"

                            echo api_config_path

                            // Importar a API no API Manager
                            /c/Axway/Tools/axway-apimcli-1.14.1/apim-cli-1.14.1/scripts/apim.sh api import -c ${api_config_path} -s teste
                        }
                    } else {
                        echo "No modified swagger files found."
                    }
                }
            }
        }
    }
}
