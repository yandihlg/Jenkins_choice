node {
  checkout scm

  if (!fileExists('config.yaml')) {
    error "No encuentro config.yaml en el workspace"
  }

  // Leer YAML de forma robusta
  def cfg = readYaml text: readFile('config.yaml')
  def entornos = cfg?.config?.entornos

  if (!(entornos instanceof Map) || entornos.isEmpty()) {
    error "config.yaml debe tener config.entornos como mapa con al menos un entorno"
  }

  // Construir opciones de ENTORNO a partir de las claves
  List entornoOpts = entornos.keySet().toList().sort()

  properties([
    parameters([
      string(name: 'Usuarios', defaultValue: '', description: 'Lista de usuarios separados por coma, p.ej: user1,user2,user3'),
      choice(name: 'ENTORNO', choices: entornoOpts.join('\n'), description: 'Selecciona el entorno')
    ])
  ])

  // Si es el primer run (no hay valor aún), sembramos y salimos
  if (!params?.ENTORNO) {
    echo "Parámetros actualizados desde YAML. Vuelve a lanzar el job y verás ENTORNO con: ${entornoOpts}"
    currentBuild.result = 'SUCCESS'
    return
  }

  // Recuperar el nodo del entorno seleccionado
  def seleccionado = entornos[params.ENTORNO]
  if (seleccionado == null) {
    error "El entorno '${params.ENTORNO}' no existe en config.entornos"
  }

  // Normalizar: tu YAML define cada entorno como LISTA de MAPAS. La convertimos a MAPA.
  Map envCfg = [:]
  if (seleccionado instanceof Map) {
    envCfg = seleccionado
  } else if (seleccionado instanceof List) {
    seleccionado.each { item ->
      if (item instanceof Map) {
        envCfg.putAll(item as Map)
      } else {
        error "Formato inválido en config.entornos.${params.ENTORNO}: se esperaba mapa o lista de mapas"
      }
    }
  } else {
    error "Formato inválido en config.entornos.${params.ENTORNO}"
  }

  // Extraer variables
  def sa_token_credential = envCfg.get('sa_token_credential')
  def cluster_api         = envCfg.get('cluster_api')
  def git_branch          = envCfg.get('git_branch')
  def git_path            = envCfg.get('git_path')

  // Validaciones rápidas
  ['sa_token_credential', 'cluster_api', 'git_branch', 'git_path'].each { k ->
    if (!envCfg.get(k)) {
      error "Falta la clave '${k}' en config.entornos.${params.ENTORNO}"
    }
  }

  stage('Imprimir variables del entorno') {
    echo "ENTORNO seleccionado: ${params.ENTORNO}"
    echo "sa_token_credential: ${sa_token_credential}"
    echo "cluster_api        : ${cluster_api}"
    echo "git_branch         : ${git_branch}"
    echo "git_path           : ${git_path}"
  }

  // Aquí ya puedes usar las variables en más stages...
}
