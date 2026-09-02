# 03. Depuración con Git Bisect (Git Bisect Debugging)

`git bisect` es una herramienta de depuración que utiliza un algoritmo de búsqueda binaria para encontrar el commit exacto que introdujo un error o regresión (*bug*) en el código. Al reducir la búsqueda logarítmicamente ($O(\log n)$), permite aislar un fallo entre cientos de commits en solo unos pocos pasos.

---

## 1. Concepto y Funcionamiento

En lugar de revisar cada commit uno por uno de forma lineal:
1. Marcas un estado conocido como **malo** (`bad`), usualmente el commit actual donde el bug es presente.
2. Marcas un estado antiguo conocido como **bueno** (`good`), donde estás seguro de que el código funcionaba correctamente.
3. Git selecciona automáticamente un commit intermedio (*punto medio*) y te ubica en él para que pruebes si el bug persiste o no.
4. Repites la clasificación (`good` o `bad`) hasta que Git identifique de forma unívoca la confirmación culpable.

---

## 2. Flujo de Trabajo Manual Paso a Paso

### Paso 1: Iniciar la Sesión de Bisect
```bash
git bisect start
```

### Paso 2: Definir los Limites de Búsqueda
```bash
# Marcar la versión actual (HEAD) como defectuosa
git bisect bad

# Marcar un commit o etiqueta anterior donde todo funcionaba
git bisect good v1.0.0
# O pasando el hash directamente:
git bisect good 4f2a89b
```

*Salida de ejemplo:*
```text
Bisecting: 8 revisions left to test after this (roughly 3 steps)
[a1b2c3d4e5f6] fix: actualizar archivo de configuración
```

### Paso 3: Probar el Código e Informar a Git
En cada paso intermedio, compila y prueba el sistema. Según el resultado, informa a Git:

```bash
# Si el bug persiste en esta revisión:
git bisect bad

# Si el código funciona correctamente en esta revisión:
git bisect good
```

### Paso 4: Causa Raíz Encontrada
Una vez aislado el problema, Git imprimirá la información completa del commit causante:

```text
a1b2c3d4e5f6g7h8i9j0 es el primer commit con problemas
Author: Desarrollador <dev@ejemplo.com>
Date:   Mon Sep 1 10:00:00 2026 -0600

    refactor: optimizar bucle de lectura del temporizador
```

### Paso 5: Finalizar y Salir
Restaura tu repositorio al estado y rama previa a la depuración:

```bash
git bisect reset
```

---

## 3. Depuración Automatizada con `git bisect run`

Puedes automatizar todo el proceso si cuentas con un script, prueba unitaria o ejecutable que devuelva un código de salida (*exit code*):
* `0`: El código es **bueno** (éxito).
* `1` a `127` (excepto 125): El código es **malo** (fallo).
* `125`: No se puede probar la revisión (Git omitirá el commit usando `git bisect skip`).

### Ejemplo de Ejecución Automática:

```bash
# Iniciar y definir límites
git bisect start
git bisect bad HEAD
git bisect good v1.0.0

# Ejecutar la prueba automática (script de prueba, makefile, test runner)
git bisect run pytest tests/test_adc_driver.py
# O usando un script de shell personalizado:
git bisect run ./scripts/check_build.sh
```

Git ejecutará el script en cada iteración de forma autónoma hasta encontrar el commit defectuoso en cuestión de segundos.

---

## 4. Opciones y Comandos Útiles

| Comando | Descripción |
| :--- | :--- |
| **`git bisect skip`** | Omite el commit actual si no se puede compilar o probar en ese punto. |
| **`git bisect log`** | Muestra el historial de marcados (`good`/`bad`) en la sesión actual. |
| **`git bisect visualize`** | Muestra gráficamente los commits restantes por evaluar (requiere `gitk` o cliente gráfico). |
| **`git bisect reset <branch>`** | Sale del modo bisect y te ubica directamente en una rama específica. |