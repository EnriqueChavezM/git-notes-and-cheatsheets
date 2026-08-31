# 05. Uso de Stash en Git

`git stash` es un área de almacenamiento temporal (un *borrador* o *guardado rápido*) que permite guardar el trabajo en progreso (*Work in Progress* o WIP) sin tener que hacer un commit incompleto. Es ideal para limpiar el directorio de trabajo cuando necesitas cambiar de rama de forma urgente para solucionar un bug o actualizar tu código.

---

## 1. ¿Cuándo usar `git stash`?

* Necesitas cambiar a otra rama rápidamente (ej. atender un `hotfix`), pero tus cambios actuales en la rama activa están incompletos.
* Quieres sincronizar la rama actual con `git pull` pero tienes modificaciones locales que entran en conflicto.
* Quieres probar un enfoque de código distinto sin perder las modificaciones actuales.

---

## 2. Comandos Básicos de Stash

### Guardar Cambios
Guarda las modificaciones de los archivos rastreados (*tracked*) y devuelve el directorio de trabajo a un estado limpio:

```bash
# Guardado rápido por defecto
git stash

# Guardado con un mensaje descriptivo (Recomendado)
git stash save "wip: implementacion inicial del driver spi"
# O en versiones recientes de Git:
git stash push -m "wip: implementacion inicial del driver spi"
```

### Incluir Archivos No Rastreados o Ignorados
Por defecto, `git stash` ignora archivos nuevos no rastreados (*untracked*) y archivos dentro de `.gitignore`.

```bash
# Incluir archivos no rastreados (untracked)
git stash -u

# Incluir absolutamente todo (archivos no rastreados e ignorados)
git stash -a
```

---

## 3. Inspeccionar y Administrar la Pila (Stash List)

El stash funciona como una pila (LIFO: *Last In, First Out*). Puedes almacenar múltiples borradores en la lista.

### Listar los Stashes Guardados
```bash
git stash list
```
*Salida de ejemplo:*
```text
stash@{0}: On feature/spi: wip: implementacion inicial del driver spi
stash@{1}: On main: fix rápido en calibración adc
```

### Ver el Contenido de un Stash
Muestra las diferencias (*diff*) guardadas en un stash específico sin aplicarlo:

```bash
# Ver cambios del stash más reciente
git stash show -p

# Ver cambios de un stash específico
git stash show -p stash@{1}
```

---

## 4. Recuperar y Aplicar Cambios

### Aplicar y Eliminar del Stash (`pop`)
Recupera los cambios del stash más reciente (o especificado) y lo elimina de la pila:

```bash
# Aplicar el último stash (stash@{0}) y eliminarlo
git stash pop

# Aplicar un stash específico y eliminarlo
git stash pop stash@{1}
```

### Aplicar sin Eliminar (`apply`)
Recupera los cambios pero los mantiene guardados en la pila de stash:

```bash
# Aplicar el último stash manteniendo la copia guardada
git stash apply

# Aplicar un stash específico manteniendo la copia
git stash apply stash@{1}
```

---

## 5. Limpieza y Eliminación de Stashes

### Eliminar un Stash Específico
```bash
git stash drop stash@{0}
```

### Vaciar Toda la Pila de Stashes
Elimina de forma permanente todos los elementos del stash:

```bash
git stash clear
```

---

## 6. Crear una Rama a partir de un Stash

Si los cambios guardados en el stash entran en conflicto con la rama actual, puedes crear una nueva rama limpia directamente desde el stash:

```bash
git stash branch feature/driver-spi-v2 stash@{0}
```

Esto crea la rama `feature/driver-spi-v2`, aplica el stash `stash@{0}` y lo elimina automáticamente de la pila si no hubo conflictos.