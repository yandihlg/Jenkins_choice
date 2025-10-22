node {
  checkout scm
  def cfg = readYaml file: 'config.yaml'
  def choicesList = cfg.opciones as List
  properties([
    parameters([
      choice(name: 'ENTORNO', choices: choicesList.join('\n'), description: 'Desde YAML')
    ])
  ])

  stages {
    stage('Checkout (este repo)') {
      steps {
        sh echo "echo 'Has elegido el entorno: ${params.ENTORNO}'"
      }
    }
  }
}
