node {
  checkout scm
  def cfg = readYaml file: 'config/opciones.yaml'
  def choicesList = cfg.opciones as List
  properties([
    parameters([
      choice(name: 'ENTORNO', choices: choicesList.join('\n'), description: 'Desde YAML')
    ])
  ])
  // … resto del pipeline
}
