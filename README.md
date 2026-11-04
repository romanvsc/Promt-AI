# Promt-AI

Prompt maestro y kit reutilizable para iniciar proyectos con arquitectura hexagonal y DDD, tres skills, plantillas, validador y visor del plan maestro.

El agente consulta las fuentes, los requisitos y el stack al comenzar cada proyecto. Reutiliza las decisiones ya informadas y puede trabajar con varios stacks por componente. Hay perfiles base, Laravel, Flutter y Go. La raíz del código propio es `/src` en la raíz del repositorio, siempre salvo elección expresa del usuario. El arranque prepara esa raíz, arquitectura, skills, plan y visor; las funcionalidades se desarrollan cuando el usuario las solicita.

## Empezar un proyecto

1. Abrí el [prompt maestro](kit-inicio-proyecto/PROMPT_MAESTRO.md).
2. Adjuntá el [ZIP del kit](dist/kit-inicio-proyecto.zip) o la carpeta `kit-inicio-proyecto` al chat del nuevo proyecto.
3. Pegá el prompt e indicá los datos que ya conocés. El agente consulta las fuentes y el stack que falten antes de cerrar el relevamiento y aplicar un perfil.

La [guía del kit](kit-inicio-proyecto/README.md) explica la instalación, el formato del plan, los comandos y sus límites.

## Instalación desde una copia del repositorio

Requiere Python 3.9 o posterior; las herramientas usan su biblioteca estándar.

```sh
git clone https://github.com/wj-004/Promt-AI.git
cd Promt-AI/kit-inicio-proyecto
python3 -B scripts/init_project.py --target /ruta/al/proyecto --name "Mi proyecto" --profile base --dry-run
```

Cuando se hayan revisado los destinos, repetí el comando sin `--dry-run`. El perfil `base` conserva el stack pendiente de definición. El instalador prepara `src/.gitkeep`, no sobrescribe archivos diferentes ni genera código funcional del producto. Una ubicación distinta requiere elección expresa del usuario y la opción `--source-root`; el framework no decide esa excepción.

Las fuentes pueden ser adjuntos, enlaces, texto del chat o archivos en su ubicación actual. `docs/fuentes/README.md` es el índice que se instala en el proyecto, con la consulta pendiente hasta registrar tu respuesta. No hay que depositar los documentos en una carpeta para que el agente los solicite o analice.

## Skills incluidas

| Skill | Función |
| --- | --- |
| [arquitectura-hexagonal-ddd](kit-inicio-proyecto/skills/arquitectura-hexagonal-ddd/SKILL.md) | Reglas del núcleo, puertos, adaptadores, contextos y perfiles tecnológicos. |
| [plan-maestro-desarrollo-sistema](kit-inicio-proyecto/skills/plan-maestro-desarrollo-sistema/SKILL.md) | Relevamiento, alcance, tareas, criterios de aceptación y evidencia. |
| [manage-project-roadmap](kit-inicio-proyecto/skills/manage-project-roadmap/SKILL.md) | Validación, consultas, exportación y visor interactivo del plan. |

## Visor del plan maestro

El visor ofrece búsqueda, filtros, detalle de tareas, zoom y cadenas de dependencias. Puede exportarse como HTML o sincronizarse con los documentos mediante un servidor local.

![Visor del plan maestro en un proyecto de demostración](docs/visor-plan-maestro.jpg)

## Verificación

Desde `kit-inicio-proyecto`:

```sh
python3 -B -m unittest discover -s tests -v
node --check skills/manage-project-roadmap/assets/roadmap-tree.js
```

Node se usa para comprobar la sintaxis JavaScript; no es una dependencia de ejecución del kit. Las pruebas HTTP requieren permitir servidores temporales en loopback.

El ZIP contiene los mismos 33 recursos del kit y un `MANIFEST.sha256`. Para regenerarlo:

```sh
python3 -B scripts/package_kit.py --output ../dist/kit-inicio-proyecto.zip
```
