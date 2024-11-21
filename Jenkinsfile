pipeline {
    agent any
    parameters {
        string(name: 'TEXTO_INDEX', defaultValue: 'TESTE', description: 'Texto da index')
    }
    stages {
        stage('Criando Arquivo Index'){
            steps {
                echo "Criando arquivo com o arquivo ${params.TEXTO_INDEX}"
                sh """ 
                echo "${env.TEXTO_INDEX}" > index.html 
                """
            }
        }
        stage('Build da imagem'){
            steps {
                script{
                    echo "Buscando imagem"
                    echo 'construindo imagem'
                    sh 'docker build -t testep1 .'
                    sh 'docker image prune -f'
                }
            }
        }
        stage('Iniciando o container'){
            steps{
                script {
                    echo 'Verificando existencia do container'
                    def containerExiste = sh(script: "docker ps -a --filter 'name=testepiperun' --format '{{.Names}}'", returnStdout: true).trim()
                    if (containerExiste) { 
                        echo 'Container existe, excluindo...'
                        sh """
                            docker stop testepiperun
                            docker rm testepiperun
                        """
                    }
                    echo 'Iniciando o container'
                    sh 'docker run --name testepiperun -dp 8081:80 testep1'  
                }
            }
        }
    }
}
