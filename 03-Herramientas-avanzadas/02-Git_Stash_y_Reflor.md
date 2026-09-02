# 02. Git Stash Avanzado y Reflog

Esta guía cubre técnicas avanzadas para gestionar cambios temporales con `git stash` y el uso de `git reflog`, el mecanismo de registro interno de Git que actúa como red de seguridad para recuperar estados, commits o ramas aparentemente perdidas.

---

## 1. Operaciones Avanzadas con `git stash`

Además del flujo básico de guardado y aplicación, `git stash` permite un control más preciso sobre los archivos y fragmentos de código que deseas enviar al borrador.

### Guardado Parcial de Cambios (`git stash push -p`)
Permite seleccionar interactiva y manualmente qué bloques (*hunks*) de código enviar al stash y cuáles mantener en el área de trabajo.

```bash
# Iniciar la selección interactiva de fragmentos
git stash push -p -m "wip: solo refactor de funciones auxiliares"
```

### Crear una Rama desde un Stash Conflictuado
Si intentas ejecutar `git stash pop` y surgen conflictos severos con tu rama actual, puedes crear una rama limpia basada en el commit donde se originó el stash:

```bash
# Crear e ir a la nueva rama con el contenido del stash aplicado
git stash branch feature/recuperada stash@{0}
```

---

## 2. Diagnóstico y Registro con `git reflog`

`git reflog` (*Reference Log*) registra cronológicamente cada movimiento del puntero `HEAD` en tu repositorio local (cambios de rama, commits, rebases, merges, resets, etc.).

> **Nota:** `reflog` es una herramienta puramente local. Sus registros no se envían al repositorio remoto al hacer `git push`.

### Visualizar el Historial de Referencias

```bash
# Ver el registro de movimientos de HEAD
git reflog

# Ver el registro de referencias de una rama específica
git reflog show main
```

*Salida de ejemplo:*
```text
7b3f12a HEAD@{0}: reset: moving to HEAD~1
a9d8e7c HEAD@{1}: commit: feat: agregar driver i2c
3f5a2b1 HEAD@{2}: checkout: moving from main to feature/sensor
```

---

## 3. Escenarios de Rescate y Recuperación

### Caso 1: Recuperar un Commit Eliminado por Error (`git reset --hard`)

Si ejecutaste un `git reset --hard` no deseado y perdiste uno o varios commits:

1. **Localizar el hash del commit:**
   ```bash
   git reflog
   ```
   *Busca la línea previa al reset, por ejemplo `a9d8e7c HEAD@{1}: commit: feat: agregar driver i2c`.*

2. **Restaurar la rama a ese punto:**
   ```bash
   git reset --hard a9d8e7c
   ```

---

### Caso 2: Recuperar una Rama Eliminada

Si eliminaste accidentalmente una rama local con `git branch -D feature/adc`:

1. Identifica el último commit en el que se encontraba la rama consultando `git reflog`.
2. Recrea la rama a partir del Hash encontrado:
   ```bash
   git branch feature/adc a9d8e7c
   ```

---

### Caso 3: Deshacer un Rebase Interactivo Fallido

Si una sesión de rebase salió mal o destruyó el orden deseado de commits:

```bash
# Consultar el estado antes de iniciar el rebase
git reflog

# Buscar la línea similar a: HEAD@{N}: rebase (start): checkout main
# Restaurar la rama al estado exacto previo al rebase
git reset --hard HEAD@{N}
```

---

## 4. Resumen de Comandos

| Comando | Descripción |
| :--- | :--- |
| **`git stash push -p`** | Guarda partes específicas (*hunks*) de código en el stash. |
| **`git stash branch <nombre>`** | Crea una rama nueva a partir de un stash y lo aplica sin conflictos. |
| **`git reflog`** | Lista el historial completo de movimientos de `HEAD` en el repositorio local. |
| **`git reset --hard <HASH>`** | Restaura el repositorio al estado exacto registrado en el reflog. |