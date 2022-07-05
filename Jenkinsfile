pipeline {
  agent any

  parameters{
        choice(
            name: 'tipo_de_operacao',
            choices: ['sub','mul','add','div'],
            ),
        string(
            name: 'num1',
            defaultValue: '0',
            description: 'Num 1'
            ),
        string(
            name: 'num2',
            defaultValue: '0',
            description: 'Num 2')
  }

  stages {
    stage('Stage 1: Compilar projecto') {
      steps {
        bat pip install pyinstaller
        bat pip install junit-xml
        bat python -m py_compile calculadora.py
        bat pyinstaller --onefile calculadora.py
        bat 'move dist\\calculadora.exe calculadora.exe'

      }
    }
    stage('Stage 2: Executar calculadora') {
      steps {
          bat calculadora.exe ' + params.num1 + ' ' + params.num2 + ' ' + params.tipo_de_operacao + '> out.txt'
      }
    }
    stage('Stage 3: Preparar release') {
      steps {
          bat 'python -m zipfile -c delivery.zip out.txt'
      }
    }
  }
}

    post {
        always{
            archiveArtifacts artifacts: 'delivery.zip',
            onlyIfSuccessful: true
        }

    }
}