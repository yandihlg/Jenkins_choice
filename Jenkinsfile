node {
  checkout scm
  // Requiere el plugin Pipeline Utility Steps
  def cfg = readYaml file: 'config.yaml'
  def choicesList = (cfg.opciones as List).join('\n')

  properties([
    parameters([
      choice(name: 'ENTORNO', choices: choicesList, description: 'Desde YAML')
    ])
  ])

  echo "Parámetros actualizados desde YAML. Vuelve a lanzar el job y ya podrás elegir ENTORNO."
  // Opcionalmente, termina aquí para evitar usar params vacíos en este primer run:
  currentBuild.result = 'SUCCESS'
  return
}
