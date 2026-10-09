---
title: "UD 1 - Solucionario: GitHub"
description: "Solucionario con enunciados de los ejercicios de la UD 1 de EDES (GitHub)."
authors:
    - Ismael Velasco
date: 2026-10-09
icon: "material/checkbox-marked-circle-outline"
permalink: /edes/unidad1/solucionario
categories:
    - EDES
tags:
    - EDES
    - GitHub
    - Git
---

# Solucionario UD 1 — GitHub

!!! warning "Uso responsable"
    El solucionario existe para **autoevaluarse después de intentarlo**. Si copias sin hacer los ejercicios, el examen lo delata: los ejercicios prácticos (Bloques B y C) se evalúan en el propio repositorio, con su historial de commits a la vista.

## Bloque A — Conceptos

**A1.** **Git** es el sistema de control de versiones: un programa que se ejecuta en tu ordenador y guarda el historial de cambios (nace en 2005, autor Linus Torvalds). **GitHub** es un servicio web que aloja repositorios Git en la nube y añade colaboración: PRs, issues, Actions, Pages.

*Git sin GitHub: sí, perfectamente — Git funciona en local (incluso sin internet). GitHub sin Git: no, GitHub es un "host" de repositorios Git; sin Git no hay repos que alojar.*

**A2.** Los tres estados: **1) Working directory** (tu carpeta, editas libremente) → `git add` → **2) Staging area** (sala de espera con los cambios elegidos) → `git commit` → **3) Repositorio local** (historial en tu disco) → `git push` → **4) Repositorio remoto** (GitHub). En sentido inverso, `git pull` trae del remoto y integra.

**A3.** `git add README.md` prepara **un fichero concreto**; `git add .` prepara **todos los cambios del directorio actual** (nuevos, modificados, borrados). Conviene el específico cuando quieres un commit limpio y temático (solo lo relacionado con ese cambio); el `.` es cómodo y válido al final del día o cuando todo lo cambiado pertenece al mismo cambio lógico. Riesgo del `.`: colar ficheros no deseados si no hay buen `.gitignore`.

**A4.** El **SHA** es el identificador único (hash) de cada commit; en corto se usan los 7 primeros caracteres (`a1b2c3d`). Sirve para referenciar commits de forma exacta: `git show a1b2c3d`, `git revert a1b2c3d`, `git checkout a1b2c3d -- fichero`. Historial compacto: `git log --oneline -5`.

**A5.**
    1. **Falso.** Git guarda deltas/cambios entre versiones (más eficiente que copiar el fichero completo).
    2. **Falso.** `git revert` crea un **commit nuevo** que deshace los cambios de otro; el que borra/reescribe historia es `git reset` (y `rebase`), y en ramas compartidas se considera peligrosa.
    3. **Verdadero.** El nombre estándar actual es `main` (antes `master`; aún puede verse en repos antiguos).
    4. **Verdadero.** Un PR sigue los commits que se suben a su rama origen, se puede comentar, corregir y actualizar hasta el merge.

**A6.** `.gitignore` enumera rutas que Git debe **ignorar** (no versionar). Ejemplos típicos: `build/`, `.idea/`, `*.log`, `.env`. Si subes un `.env` con contraseñas a un repo público, las claves quedan expuestas a cualquiera (bots las rastrean en minutos); la limpieza posterior **no basta** porque el commit queda en el historial — hay que rotar/revocar las credenciales expuestas y reescribir la historia.

**A7.** El workflow se dispara automáticamente en **cada push** al repo y **cada PR** abierto o actualizado. Beneficio: cada propuesta se **compila/prueba en una máquina limpia** antes de fusionarse; si el check falla, el PR se marca en rojo y no se fusiona (o se revisa con cuidado). Es la integración continua evitando «en mi máquina funcionaba».

## Bloque B — Uso guiado

**B1.** Pasos: repo en GitHub (con README, público) → `git clone https://github.com/usuario/edes-u1.git` → editar `README.md` → `git add README.md` → `git commit -m "Presentación: nombre, ciclo y módulo"` → `git push`. En la web, el commit aparece con tu avatar, nombre y fecha/hora exacta (pestaña *Commits*).

**B2.** Tras los tres commits: `git log --oneline` lista la cadena en orden inverso (`HEAD` arriba). `git show <SHA>` muestra patches y metadatos. `git diff` compara working directory vs staging; también `git diff --staged` (lo ya preparado). Si editas sin `add`, `git diff` muestra esa línea cambiada en rojo/verde.

**B3.** Comprobación clave: al volver a `main`, `cat README.md` muestra el fichero **sin** la frase (la rama es otra línea de tiempo). En la rama, la frase está. `git push -u origin feature/frase-motivadora` sube la rama: GitHub ofrece el botón *Compare & pull request* porque detecta commits nuevos respecto a `main`.

**B4.** El PR se abre desde la rama hacia `main`, con *Files changed* para comentar línea a línea; el merge (botón verde) integra y GitHub propone borrar la rama. En local tras el merge: `git switch main` + `git pull` trae el merge y la frase desaparece... **aparición**: la frase ya está en `main` local.

**B5.** Conflicto esperado: Git marcará en el fichero:

```
<<<<<<< HEAD
Frase de main
=======
Frase de feature/frase-b
>>>>>>> feature/frase-b
```

Se edita el fichero dejando la versión elegida (o combinada), `git add README.md` y `git commit` para cerrar el merge. Idea clave: el conflicto **no es un error**, es Git pidiendo una decisión humana sobre dos versiones de la misma línea.

**B6.** El mensaje del commit con la palabra mágica (`fixes #N`, también `closes`/`resolves`) hace que GitHub **cierre el issue automáticamente al fusionar el PR**. Comprobación: el issue aparece como *Closed* con una referencia al commit/PR que lo cerró.

**B7.** Workflow mínimo:

```yaml
name: Hola EDES
on:
  push:
  pull_request:
jobs:
  hola:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Hola desde Actions"
```

Para `pull_request`: añadir la línea `pull_request:` bajo `on:` (o usar `on: [push, pull_request]`).

**B8.** Con `.gitignore` conteniendo `secretos.txt`, `git add .` no lo incluye y `git status` no lo lista (Git lo ignora). Con `build/` sucede igual para toda la carpeta. Matiz útil: `.gitignore` no ignora ficheros **ya rastreados**; si algo se subió antes, hay que sacarlo con `git rm --cached`.

## Bloque C — Mini-proyectos

**C1.** Claves de la solución: repo público `mi-portfolio` con `index.html` en la raíz; Pages activado en *Settings → Pages*, Source *Deploy from a branch*, Branch `main` carpeta `/root`; la URL pública queda `https://usuario.github.io/mi-portfolio/`. La sección «Proyectos» debe llegar por rama+PR+merge; Pages se redespliega solo tras cada merge (verlo en la pestaña *Actions* con el run de `pages build and deployment`).

**C2.** Protección clave (*Settings → Branches → Add branch protection rule*): Branch name pattern `main`; activar *Require a pull request before merging*. Con eso, un push directo devuelve el rechazo remoto `remote: error: GH006: Protected branch update failed` (o equivalente) — GitHub exige que el cambio llegue vía PR. Para desbloquear: el PR normal (rama `feature/...`, push, PR, merge).

**C3.** El workflow referencia:

```yaml
- name: Validar informes
  run: |
    faltan=$(grep -L "alumno" informes/*.txt || true)
    if [ -n "$faltan" ]; then echo "Falta 'alumno' en: $faltan"; exit 1; fi
```

El `grep -L` lista ficheros que **no** contienen la palabra; si hay alguno, `exit 1` pone el job en rojo y el commit/PR queda marcado como fallido. Rol del CI: el error se detecta en el push, no semanas después (ni en el examen).

**C4.** (Reto individual, no hay una única solución.) Checklist de evaluación: ✓ fork propio, ✓ rama descriptiva (`docs/fix-typo-readme`), ✓ commit con mensaje claro, ✓ PR abierto al original con descripción en inglés o idioma del proyecto, ✓ capturas del proceso.Bonus si se fusiona y el proyecto agradece la contribución.
