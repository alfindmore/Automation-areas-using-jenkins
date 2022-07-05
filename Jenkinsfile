pipeline {
  agent any

  parameters{
        choice(
            name: 'operacao',
            choices: ['sub','mul','add','div']
            ),
        string(
            name: 'numero1',
            defaultValue: '0'
            ),
        string(
            name: 'numero2',
            defaultValue: '0'
            )
  }

  stages {
    stage('Stage 1: Compilar projecto') {
      steps {
        bat pip install pyinstaller
        bat pip install junit-xml
        bat python -m py_compile calculadora.py
        bat pyinstaller --onefile calculadora.py

      }
    }
    stage('Stage 2: Executar calculadora') {
      steps {
          bat "calculadora.exe ${params.numero1} ${params.numero2} ${params.operacao}> out.txt""
      }
    }
    stage('Stage 3: Preparar release') {
      steps {
          bat 'python -m zipfile -c outcalculadora.zip out.txt'
      }
    }
  }
}

    post {
        always{
            archiveArtifacts artifacts: 'outcalculadora.zip',
            onlyIfSuccessful: true
        }

    }
}