node {
  checkout scm

  if (!fileExists('config.yaml')) {
    error "No encuentro config.yaml en el workspace"
  }

  // Lee el YAML (usa text para evitar sorpresas de ruta)
  def cfg = readYaml text: readFile('config.yaml')

  // Normaliza a lista independiente de la forma del YAML
  List opts = []
  if (cfg instanceof Map && cfg.containsKey('opciones')) {
    opts = (cfg.opciones ?: []) as List
  } else if (cfg instanceof List) {
    opts = cfg as List
  } else {
    error "config.yaml debe ser una lista o un mapa con la clave 'opciones'"
  }

  // Limpieza y validación
  opts = opts.collect { it?.toString()?.trim() }.findAll { it }
  if (opts.isEmpty()) {
    error "La lista de opciones está vacía en config.yaml"
  }

  properties([
    parameters([
      choice(name: 'ENTORNO', choices: opts.join('\n'), description: 'Desde YAML')
    ])
  ])

  echo "Parámetros actualizados desde YAML. Vuelve a lanzar el job y verás ENTORNO."
  currentBuild.result = 'SUCCESS'
  return
}
