# Kit de inicio de proyectos

Base para preparar proyectos con arquitectura hexagonal y DDD, consultar el stack y mantener un plan maestro con visor. Requiere Python 3.9 o posterior para las herramientas; no requiere paquetes Python ni un framework de producto. El perfil se elige con el usuario al iniciar cada proyecto.

## Uso habitual

1. Copiar o adjuntar esta carpeta completa, o su ZIP, en el chat del proyecto nuevo.
2. Pegar [PROMPT_MAESTRO.md](PROMPT_MAESTRO.md) y completar los datos conocidos.
3. El agente revisa las fuentes, consulta requisitos y stack pendientes y prepara el proyecto. El código funcional empieza cuando se lo solicites.

El prompt contiene la especificación suficiente para generar una base compatible si el kit no está adjunto; los recursos incluidos permiten reutilizar una implementación probada del validador y el visor.

## Instalación de los recursos

Desde esta carpeta, primero podés ver lo que se crearía:

```sh
python3 -B scripts/init_project.py --target /ruta/al/proyecto --name "Mi proyecto" --profile base --dry-run
```

Para crearlo:

```sh
python3 -B scripts/init_project.py --target /ruta/al/proyecto --name "Mi proyecto" --profile base
```

Perfiles disponibles: `base` (stack pendiente), `laravel`, `flutter` y `go`. Son guías de adaptación, no selección automática de lenguaje, versión, UI, ORM o proveedor. Cuando el usuario confirma una combinación distinta, el agente completa el perfil local respetando las reglas comunes.

Un proyecto puede combinar stacks. `--profile` indica la guía inicial de instalación; la ficha conserva el stack de cada componente y la skill de arquitectura consulta los perfiles que correspondan. No obliga a usar el mismo lenguaje en backend, cliente y herramientas.

El instalador no sobrescribe archivos diferentes, no sigue enlaces simbólicos en sus destinos y comprueba todas las colisiones antes de escribir. Una repetición con la misma configuración conserva los archivos idénticos. Si ya existe `AGENTS.md`, un plan u otra skill con contenido distinto, informa las rutas en conflicto sin crear un proyecto parcial; el agente debe conciliar esa instalación existente. No instala dependencias del producto ni genera sus carpetas de código.

## Recursos instalados

```text
AGENTS.md
.agents/project.json
.agents/project-kit.json
.agents/skills/arquitectura-hexagonal-ddd/
.agents/skills/plan-maestro-desarrollo-sistema/
.agents/skills/manage-project-roadmap/
docs/proyecto.md
docs/arquitectura/README.md
docs/decisiones/README.md
docs/plan-maestro/README.md
docs/plan-maestro/00-preparacion.md
docs/plantillas/
```

Se preparan tres tareas iniciales de relevamiento/arquitectura/plan, todas pendientes. No hay requisitos de negocio ficticios ni funcionalidades declaradas terminadas. Las decisiones, módulos y tareas reales se agregan al analizar el proyecto. Las skills globales y los proyectos que sirvieron de referencia permanecen fuera de esta instalación.

## Comandos desde el proyecto preparado

```sh
python3 -B .agents/skills/manage-project-roadmap/scripts/roadmap.py check
python3 -B .agents/skills/manage-project-roadmap/scripts/roadmap.py report --status EN_CURSO
python3 -B .agents/skills/manage-project-roadmap/scripts/roadmap.py report --json
python3 -B .agents/skills/manage-project-roadmap/scripts/roadmap.py tree
python3 -B .agents/skills/manage-project-roadmap/scripts/roadmap.py serve --port 8788
```

El HTML se exporta a `docs/plan-maestro/visualizacion.html`. El servicio local imprime su URL; `--port 0` solicita un puerto libre al sistema y Ctrl+C lo detiene. Las opciones `--root`, `--plan-dir` y `--output` permiten usar otros directorios sin mover los scripts. No se sirven archivos del repositorio mediante un servidor de directorios genérico.

Los scripts de esta carpeta usan el mismo código que se instala. Para probar todo el kit:

```sh
python3 -B -m unittest discover -s tests -v
node --check skills/manage-project-roadmap/assets/roadmap-tree.js
```

La suite incluye pruebas HTTP en loopback. En un entorno que bloquee la apertura de puertos se necesita la capacidad de ejecutar servidores locales; no hay que cambiar las comprobaciones para obtener un resultado verde.

## Formato compartido y portabilidad

La autoridad del formato está en [plan-schema.json](skills/plan-maestro-desarrollo-sistema/references/plan-schema.json); [plan-schema.md](skills/plan-maestro-desarrollo-sistema/references/plan-schema.md) explica su significado. El parser lo carga y el visor recibe sus campos y estados del parser. La versión 1 usa catorce campos, dependencias por comas y `EXCLUIDO`, sin atribuir un presupuesto inexistente.

La base deriva de las reglas y herramientas de planificación anteriores, pero elimina sus referencias de producto y unifica los esquemas. No migra automáticamente planes con nueve campos o dependencias por punto y coma. Copiar este kit a un proyecto nuevo no cambia el plan ni las skills del repositorio donde se preparó.

Para empaquetar una copia limpia:

```sh
python3 -B scripts/package_kit.py --output /ruta/kit-inicio-proyecto.zip
```

Se incluyen únicamente prompt, documentación, scripts, skills, plantillas y pruebas; no se agregan `.env`, cachés, proyectos temporales, capturas ni datos de una aplicación.
