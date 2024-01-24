pipeline {
    agent any
    tools {
          maven 'maven'
          jdk 'JDK11'
    }
    parameters {
        choice(name: 'AGENT_NODE', choices: ['any'], description: 'Seleccionar NODO')
        choice(name: 'ENV', choices: ['dev', 'uat'], description: 'Selecionar ambiente')
        string(name: 'SCENARIO_TAG', trim: false, description: 'Tag a ejecutar')
    }
    stages {
        stage('Execute Tests'){
            steps{
                script{
                    try{
                       // withMaven(maven: 'maven'){
                            if(isUnix()){
                               echo "Ejecutando tag: ${params.SCENARIO_TAG}"
                               sh "java -version"
                               sh 'mvn clean verify -Dcucumber.filter.tags="${params.SCENARIO_TAG}"'
                            }
                             else{
                                echo "Ejecutando tag: $params.SCENARIO_TAG"
                                bat "java -version"
                                bat 'mvn clean verify -Dcucumber.filter.tags="@%SCENARIO_TAG%"'
                             }
                       // }

                    } finally{
                          publishReport();
                    }
                }
            }
        }
    }
}
    def publishReport(){
        publishHTML(target: [
            reportName : 'Serenity Report',
            reportDir:   'target/site/serenity',
            reportFiles: 'index.html',
            keepAll:     true,
            alwaysLinkToLastBuild: true,
            allowMissing: false
        ])
    }