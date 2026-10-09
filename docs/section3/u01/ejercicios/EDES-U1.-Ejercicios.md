---
title: "UD 1 - Ejercicios: GitHub"
description: "Relación de ejercicios del tema GitHub, de menor a mayor dificultad: Bloque A (conceptos), Bloque B (uso guiado de Git/GitHub) y Bloque C (mini-proyectos completos)."
authors:
    - Ismael Velasco
date: 2026-10-09
icon: "material/puzzle"
permalink: /edes/unidad1/ejercicios
categories:
    - EDES
tags:
    - EDES
    - GitHub
    - Git
---

# Ejercicios UD 1 — GitHub

Los ejercicios del **Bloque A** se responden con la teoría delante (o de memoria, que es como debe ser). Para el **Bloque B** necesitas una cuenta gratis en [github.com](https://github.com) y Git instalado (`git --version`). El **Bloque C** son mini-proyectos completos que quedan de práctica evaluable.

## Bloque A — Conceptos (calentamiento)

**A1.** Explica con tus palabras la diferencia entre **Git** y **GitHub**. ¿Podrías usar Git sin GitHub? ¿Y GitHub sin Git?

**A2.** Enumera los **tres estados** por los que pasa un fichero en Git (del directorio al repositorio remoto) y el comando que lo mueve en cada paso.

**A3.** ¿Qué diferencia hay entre `git add .` y `git add README.md`? ¿Cuándo conviene usar cada uno?

**A4.** ¿Qué es un **SHA** (p. ej. `a1b2c3d`) y para qué sirve en la práctica? ¿Cómo consultarías los últimos 5 commits de un repo en formato compacto?

**A5.** Verdadero o falso (justifica la respuesta):
    1. Un commit guarda siempre el fichero completo, no los cambios.
    2. `git revert` borra el commit del historial.
    3. La rama principal de un repositorio moderno se llama `main`.
    4. Un pull request se puede seguir editando después de abrirlo.

**A6.** ¿Para qué sirve el fichero **`.gitignore`**? Pon tres ejemplos de entradas típicas y explica qué pasaría si subes a un repo público un fichero `.env` con contraseñas.

**A7.** ¿Qué significa que un workflow de **GitHub Actions** tenga `on: push` y `on: pull_request`? ¿Qué beneficio aporta a un equipo que trabaja con pull requests?

## Bloque B — Uso guiado de Git y GitHub

**B1. Tu primer repo.** Crea en GitHub un repositorio público llamado `edes-u1` con README. Clónalo en tu equipo, edita el README (tu nombre, ciclo y módulo), haz `add` + `commit` + `push`, y verifica en la web que aparece tu cambio con tu nombre y hora exactos.

**B2. Historial y diffs.** En el repo anterior, haz tres commits más con cambios pequeños y mensajes descriptivos (p. ej. añadir una línea de «objetivos», corregir una errata, añadir tu sección «Sobre mí»). Después: lista el historial con `git log --oneline`, muestra el detalle del segundo commit con `git show`, y comprueba con `git diff` qué hay pendiente si editas algo sin hacer `add`.

**B3. La rama canalla.** Desde `main`, crea la rama `feature/frase-motivadora`, cambia a ella, añade al README una frase motivadora con su commit, vuelve a `main` y comprueba con `cat README.md` que la frase **no** está ahí. Vuelve a la rama y verifica que sí está. Sube la rama a GitHub (`git push -u origin feature/frase-motivadora`) y observa que GitHub te ofrece abrir un PR.

**B4. Tu primer pull request.** Abre el PR de la rama anterior (con título y descripción), revísalo tú mismo en la pestaña *Files changed* comentando una línea, y fusiónalo con merge. Borra la rama tras el merge y verifica que `main` tiene la frase. ¿Qué pasaría si ahora haces `git pull` en tu local desde `main`?

**B5. Deriva un conflicto (a propósito).** Crea la rama `feature/frase-b` desde `main`, edita la **misma línea** del README que editaste en B3 (hazlo de nuevo en otra rama o replantéalo con otra frase) y fuerza un conflicto de merge con `main` al intentar fusionar. Resuélvelo dejando la mejor frase (o ambas), confirma y termina el merge. Documenta en el README qué conflictos encontraste y cómo los resolviste.

**B6. Issue con cierre automático.** Abre un issue titulado «Añadir sección de herramientas» (#X, apunta el número). Crea la rama `feature/herramientas`, añade al README la sección con 3 herramientas del temario (p. ej. VS Code, Android Studio, Git), y en el mensaje commit incluye `fixes #X` exacto. Haz push, abre PR, fusiónalo y comprueba que GitHub cierra el issue solo.

**B7. Blueprint:: Actions.** En el repo, crea la carpeta `.github/workflows/hola.yml` con un workflow `name: Hola EDES` que se dispare `on: push` y ejecute `echo Hola desde Actions` en `ubuntu-latest`. Haz push y revisa en la pestaña *Actions* que aparece el run con check verde. ¿Qué línea cambiarías para que también se dispare con `pull_request`?

**B8. Gitignore.** Crea un fichero `secretos.txt` con texto inventado y un fichero `.gitignore` con la línea `secretos.txt`. Haz `git add .` y comprueba con `git status` que `secretos.txt` **no** se ha añadido. BONUS: hazlo con un fichero dentro de la carpeta `build/` y la entrada `build/`.

## Bloque C — Mini-proyectos (integración)

**C1. Portfolio en GitHub Pages.** Crea el repo `mi-portfolio` con un `index.html` simple (tu nombre, ciclo y enlaces a tus redes). Sube el código a `main` y activa *Settings → Pages → Deploy from branch → main /root*. Verifica que tu web está publica en `https://tuusuario.github.io/mi-portfolio/`. Después, añade una sección «Proyectos» con un PR (rama + PR + merge) y comprueba que la web se actualiza sola.

**C2. Repo de prácticas con protección.** Crea el repo `pmdm-practicas` con una carpeta por práctica (`practica01/`, `practica02/`) y un README raíz con tu nombre. Sube el código. Después configura en GitHub *Settings → Branches → Add rule* para proteger `main` (requiere PR antes del merge). Intenta hacer `git push` directo a `main`: documenta qué error devuelve GitHub y por qué. Sube entonces una rama `feature/readme-ajustes` con un cambio del README y fusiónalo por PR — la única vía posible ahora.

**C3. Mini CI con Actions.** Partiendo del repo de C2, añade `.github/workflows/ci.yml` que en cada `push` y `pull_request` compruebe que todo fichero `.txt` de informes contiene la palabra «alumno» (script bash de 3 líneas con `grep -L`). Fusiona, rompe a propósito un fichero (renómbralo sin la palabra) y verifica que el check del commit se pone rojo. Documenta en el README qué papel jugó el CI en detectar el fallo.

**C4. Contribución real (reto).** Busca en [github.com/explore](https://github.com/explore) un proyecto open source con issues etiquetados `good first issue`. Lee el `CONTRIBUTING.md` si existe, haz **fork**, crea una rama, mejora la documentación (una errata, una frase, un ejemplo mejor) y abre un PR real al proyecto original. Aunque no se fusione, documenta el proceso completo con capturas: fork, rama, commit y PR abierto. *(Este ejercicio vale doble en la práctica evaluable.)*
