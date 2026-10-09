---
title: "UD 1 - GitHub: repositorios, versiones y trabajo en equipo"
description: "Qué es GitHub, cómo se usa Git por debajo, el flujo de trabajo con ramas y pull requests, los Issues, y las Actions que automatizan los proyectos reales."
summary: "Del commit al pull request: repositorios remotos, ramas, conflictos, issues, fork, GitHub Actions y Pages — lo básico para tener tu código en la nube y trabajar con otros."
authors:
    - Ismael Velasco
date: 2026-10-09
icon: "material/source-branch"
permalink: /edes/unidad1/github
categories:
    - EDES
tags:
    - EDES
    - GitHub
    - Git
    - Control de versiones
    - Integración continua
---

# GitHub: repositorios, versiones y trabajo en equipo

!!! abstract "Idea principal"
    GitHub es la maleta donde los programadores guardan el código, con historia y tiempo viajero incluido. Detrás hay un sistema llamado **Git** que guarda *commits* (fotos de tu proyecto), y delante una plataforma social donde colaborar: **ramas** para no romper la versión estable, **pull requests** para proponer cambios revisables, **issues** para la lista de tareas y **Actions** para que un ordenador de GitHub compile y pruebe tu app cada vez que subes algo. Al terminar esta unidad sabrás publicar tu proyecto, trabajar sobre ramas y entender cualquier repositorio profesional.

## 1. Por qué existe GitHub: el problema de la versión "final_final_v3"

Todo el mundo ha guardado versiones de un trabajo como `practica.docx`, `practica_v2.docx` y `practica_definitiva_SI.docx`. Git y GitHub resuelven ese caos de forma industrial:

| Problema sin control de versiones | Solución con Git/GitHub |
| --- | --- |
| Copiar la carpeta entera por cada cambio | Cada guardado es un **commit** con quién, cuándo y qué cambió |
| «¿Quién borró mi función?» | `git log` / `git blame` dicen el responsable exacto |
| Dos compañeros editando el mismo fichero | Cada uno trabaja en su **rama**, luego se integra sin perder nada |
| El código bueno se rompe al añadir algo nuevo | La rama `main` queda protegida; los cambios llegan revisados por **pull request** |
| Perder el trabajo si falla el PC | El repositorio vive en la nube de **GitHub** |
| Añadir a alguien al equipo | Son **cuentas y permisos**, no carpetas compartidas |

Y la clave de bóveda: **Git y GitHub no son lo mismo**. *Git* es el programa de control de versiones (nace en 2005, creado por Linus Torvalds, el autor de Linux, y funciona en tu ordenador sin internet). *GitHub* es el servicio web que aloja repositorios Git en la nube y añade las capas sociales (revisiones,Issues, Actions, Pages). Existen otros: GitLab o Bitbucket. GitHub es el más usado del mundo (más de 100 millones de desarrolladores).

## 2. Los tres estados de Git: working directory, staging y repositorio

Un fichero en un proyecto Git está en uno de **tres lugares**. Se entiende con el ciclo de vida clásico:

1. **Working directory (directorio de trabajo)** — la carpeta real con tus ficheros, donde editas sin miedo.
2. **Staging area (index / área de preparación)** — una sala de espera: `git add` mete ahí los cambios que *elegiste* y quieres formar parte del próximo commit.
3. **Repositorio (local)** — al hacer `git commit`, los ficheros del staging se fotografían y quedan en la historia con un **SHA** (identificador tipo `a1b2c3d`).

El paso a la nube es el cuarto movimiento:

```bash
git add README.md      # working directory → staging
git commit -m "Añade README"   # staging → repositorio local
git push               # repositorio local → GitHub (remoto)
```

Y en sentido contrario, `git pull` trae los cambios que hay en GitHub y los integra en tu copia. Este ida-y-vuelta (`add` → `commit` → `push`, y a veces `pull`) es el 90 % del dia a dia; el resto del vocabulario (ramas, conflictos, tags) son variantes del mismo viaje.

!!! tip "El mensaje de commit importa"
    Un commit es una foto + una nota explicativa. `git commit -m "cambios"` no explica nada a tu equipo (ni a ti mismo en tres semanas). Escribe por qué hiciste el cambio: `git commit -m "Valida la edad en el formulario antes de guardar"`. En proyectos reales, el estándar *Conventional Commits* usa prefijos como `feat:`, `fix:`, `docs:` que luego permiten generar changelogs automáticos.

## 3. Tu primer repositorio: crear, clonar y subir

Un repositorio (`repo`) es una carpeta bajo el control de Git con su historia. En GitHub, la página de inicio de casi todo es un repo. Hay dos formas de empezar, y ambas se usan a diario:

**A. Crearlo en GitHub y clonarlo (la recomendada):**

1. En [github.com](https://github.com), botón **+** → **New repository**.
2. Nombre corto descriptivo (`hola-mundo`), descripción opcional, y eliges visibilidad: **Public** (cualquiera lo ve) o **Private** (solo tú y quien invites).
3. Marca **Add a README** — ese fichero se muestra en la portada del repo y es donde se explica el proyecto.
4. Copia la URL y en tu terminal:

```bash
git clone https://github.com/tuusuario/hola-mundo.git
cd hola-mundo
```

**B. Ya tienes una carpeta local con código** — la conectas a un repo vacío creado en GitHub:

```bash
git init
git add .
git commit -m "Primer commit"
git remote add origin https://github.com/tuusuario/hola-mundo.git
git push -u origin main    # -u recuerda el destino para los push siguientes
```

!!! warning "Tus primeras credenciales: token, no contraseña"
    Desde 2021 GitHub no acepta la contraseña de la web para operaciones Git. Necesitas un **Personal Access Token (PAT)** (Settings → Developer settings → Tokens) que actúa como contraseña técnica, o usar `gh auth login` con la [GitHub CLI](https://cli.github.com). Guarda el token en un gestor de contraseñas: un token filtrado es acceso a tu cuenta entera.

Tras esos pasos, [github.com/tuusuario/hola-mundo](https://github.com) muestra tus ficheros con el historial. Cada cambio posterior repite el ciclo `git add .` → `git commit -m "..."` → `git push`.

## 4. Commits: las fotos de tu proyecto

Cada commit guarda: **quién** (autor y email), **cuándo** (fecha y hora exacta), **qué** (la diferencia respecto al commit anterior, el *diff*) y **por qué** (el mensaje). El resultado es una historia completa, navegable y auditable:

```bash
git log --oneline        # historia compacta
git show a1b2c3d         # el detalle de un commit concreto
git diff                 # qué hay sin confirmar en tu directorio
```

Un detalle crítico: Git no guarda el fichero entero en cada commit, solo los **cambios** (deltas). Por eso el historial es barato en espacio, y puedes reconstruir cualquier versión anterior sin copias manuales: `git checkout a1b2c3d -- README.md` restaura un fichero como estaba entonces. Y `git revert a1b2c3d` crea *un commit nuevo* que deshace lo que hizo aquel — la manera profesional de «volver atrás»: nunca borras historia, la contrarrestas.

## 5. Ramas: la máquina del tiempo sin sustos

La **rama** (branch) es una línea de trabajo paralela. La principal se llama `main` (anteriormente `master`) y debería contener siempre el código estable y funcional. Para arreglar un bug o desarrollar una nueva característica, naces una rama desde `main` y trabajas ahí, sin tocar la principal:

```bash
git switch -c feature/login      # crea y muévete a la rama feature/login
# ... editas, add, commit, push ...
git switch main                  # vuelves a la principal
```

Cuando la nueva funcionalidad funciona, la integras en `main` mediante un **merge** (o, mejor, una pull request, que veremos en la siguiente sección). La analogía de memoria favorita del aula: *main es el establo estrena; cada rama es la garaje donde tuneas el coche sin arrimarías al establo*.

## 6. El flujo GitHub: ramas y pull requests

El **GitHub flow** es el flujo de trabajo que GitHub recomienda para equipos (y que usan proyectos como este mismo material). Tiene 6 pasos:

1. **Crea una rama** desde `main` con nombre descriptivo (`feature/suma-pesos`, `fix/error-404`).
2. **Haz cambios y commits** en esa rama. Cada commit es un paso auditable.
3. **Abre un Pull Request (PR)**: es la petición formal de «quiero pasar mis cambios a `main`», con descripción, revisores y comentarios línea a línea sobre el *diff*.
4. **El equipo revisa**: puede comentar, sugerir cambios o pedir ajustes. Tú sigues subiendo commits y el PR se actualiza solo.
5. **Checks automáticos**: si el repo tiene GitHub Actions, corren compilación y tests*[; en muchos repos no se puede fusionar hasta que pasen]*.
6. **Merge**: aprobado el PR, se fusiona la rama en `main` y GitHub conserva toda la conversación. Después se elimina la rama.

Si dos ramas tocan la misma línea del mismo fichero, aparece un **conflicto** de merge: Git marca en el fichero las dos versiones con `<<<<<<<` y `>>>>>>>`, y una persona elige la buena (o combina ambas) y confirma. Los conflictos son normales y la forma de evitarlos gran parte es comunicarse: ramas pequeñas,(PR) muchas, que viven poco.

Con mismos repositorios colaborativos, otra pieza existe: el **fork**. Un fork es una copia del repo *en tu cuenta* propia (no editable por tí como el original). El flujo *fork & pull* es el que usan proyectos públicos (Kotlin, dozens libraries...) para aceptar contribuciones de gente sin permiso de escritura: haces fork, trabajas en tu copia, abres PR hacia el original.

## 7. Issues, proyectos y documentación: la casa en orden

Un repo profesional no es solo código:

- **Issues**: la lista de tareas, bugs y mejoras. Se numeran (`#12`), se etiquetan (`bug`, `enhancement`), se asignan. Mencionar `#12` en un commit o PR *lo vincula*; decir `fixes #12` **cierra el issue automáticamente** al fusionar.
- **Labels y milestones**: agrupan y ordenan el trabajo por versiones.
- **Wiki y README**: documentación de uso, instalación y decisiones técnicas.
- **Releases y tags**: versiones editas (`v1.0`, `v2.3.1`) con notas; es lo que descargan los usuarios.

## 8. GitHub Actions: la fábrica que trabaja mientras duermes

**GitHub Actions** es el sistema de integración continua (CI/CD) integrado en GitHub. Un *workflow* es un fichero YAML en `.github/workflows/` con tres piezas: **evento** (qué dispara), **jobs** (trabajos) y **steps** (pasos concretos):

```yaml
name: Compilar Kotlin
on:
  push:            # se dispara al subir cambios
  pull_request:    # y también al abrir PRs

jobs:
  build:
    runs-on: ubuntu-latest      # máquina virtual con Linux
    steps:
      - uses: actions/checkout@v4          # descarga el código
      - uses: actions/setup-java@v4        # instala Java
        with:
          distribution: temurin
          java-version: 21
      - run: ./gradlew build               # compila y testea
```

Con este fichero, cada push y cada PR ejecuta la compilación en una máquina limpia: si alguien rompe el código, la marca ✗ aparece en el PR *antes* de fusionar. Ese es, literalmente, el *punto por el que se factura* la integración continua: detectar roturas temprano. Los mismo workflows pueden publicar un sitio (**GitHub Pages**), desplegar una app o generar release. En este material el sitio de teoría se publica exactamente así: push → build → página web visible.

## 9. GitHub Pages y gitignore: detalles que separan amateur de pro

- **`.gitignore`**: fichero que enumera qué NO debe entrar en el repo ( compiles, secretos, `build/`, `.idea/`, credenciales). Sin él acabas subiendo MB innecesarios… o con claves públicas: **el token de acceso o el `.env` jamás se suben**.
- **GitHub Pages**: hosting gratuito de webs estáticas ligado a un repo. El material de este curso vive instalado ahí: cada push a `main` regenera el sitio con MkDocs y lo publica.
- **Licencia**: un repo sin licencia no permite a nadie usar tu código legalmente. MIT (permisiva) y GPL (copia izquierda) son las más comunes en FP.

## 10. Resumen

- **Git** versiona; **GitHub** aloja, colabora y automatiza. No es lo mismo, y aprendiste de ambos.
- El ciclo diario: `add → commit → push` y `pull` para sincronizar.
- Los commits son fotos completas con autor, fecha, mensaje y SHA.
- Las **ramas** aíslan el trabajo; el **GitHub flow** (rama → PR → revisión → merge) es el estándar del sector.
- Los **issues** convierten el proyecto en una lista de tareas trazable.
- **Actions** ejecuta builds y tests ante cada cambio, y **Pages** publica la documentación.

## Bibliografía y fuentes

- GitHub Docs. *Quickstart for repositories*. https://docs.github.com/en/repositories/creating-and-managing-repositories/quickstart-for-repositories
- GitHub Docs. *GitHub flow*. https://docs.github.com/en/get-started/using-github/github-flow
- GitHub Docs. *Documentación de GitHub Actions*. https://docs.github.com/en/actions
- Chacon, S. & Straub, B. (2014). *Pro Git* (2.ª ed.). Apress. Libre en https://git-scm.com/book
- Trabajar con GitHub CLI. https://cli.github.com
