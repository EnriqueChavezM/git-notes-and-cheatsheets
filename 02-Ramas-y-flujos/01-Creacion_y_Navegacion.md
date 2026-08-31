# 01. Creación y Navegación de Ramas

Las ramas (*branches*) son punteros móviles que apuntan a confirmaciones (*commits*) específicas. Permiten aislar el trabajo en líneas de desarrollo independientes dentro del mismo repositorio.

---

## 1. Creación de Ramas (`git branch`)

### Crear una Rama desde el Commit Actual

Para crear una nueva rama basada en el estado actual de la rama activa:

```bash
git branch feature/nueva-funcionalidad
```

### Crear una Rama desde un Commit o Rama Específica

Puedes indicar un *hash* de commit, una etiqueta (*tag*) o el nombre de otra rama para partir desde ese punto exacto del historial:

```bash
# Crear una rama a partir de un commit específico
git branch fix/error-sensor 7a8b9c1

# Crear una rama a partir de la rama main
git branch feature/modulo-lectura main
```

---

## 2. Navegación entre Ramas (`git switch` / `git checkout`)

### `git switch` (Comando Recomendado)

Git introdujo `git switch` en la versión 2.23 para separar las tareas de navegación entre ramas de las tareas de restauración de archivos.

```bash
# Cambiar a una rama existente
git switch feature/nueva-funcionalidad

# Crear una nueva rama y cambiar a ella inmediatamente
git switch -c feature/nueva-funcionalidad

# Crear una nueva rama a partir de un commit específico y cambiar
git switch -c fix/error-sensor 7a8b9c1
```

### `git checkout` (Sintaxis Tradicional)

Aunque `git switch` es el estándar moderno, es común encontrar `git checkout` en proyectos existentes y documentación antigua.

```bash
# Cambiar a una rama existente
git checkout feature/nueva-funcionalidad

# Crear una nueva rama y cambiar a ella inmediatamente
git checkout -b feature/nueva-funcionalidad
```

---

## 3. Inspección y Listado de Ramas

### Listar Ramas Locales y Remotas

```bash
# Listar ramas locales (* indica la rama activa)
git branch

# Listar ramas con información del último commit
git branch -v

# Listar todas las ramas (locales y remotas)
git branch -a

# Listar únicamente ramas remotas rastreadas
git branch -r
```

### Filtrar Ramas por Estado de Fusión

```bash
# Ver ramas que ya han sido fusionadas con la rama actual
git branch --merged

# Ver ramas que contienen cambios aún no fusionados
git branch --no-merged
```

---

## 4. Renombrado y Eliminación de Ramas

### Renombrar Ramas

```bash
# Renombrar la rama activa actualmente
git branch -m nuevo-nombre-rama

# Renombrar una rama específica estando en otra
git branch -m nombre-viejo nombre-nuevo
```

### Eliminar Ramas

```bash
# Eliminar una rama local de forma segura (solo si sus cambios ya se fusionaron)
git branch -d feature/nueva-funcionalidad

# Forzar la eliminación de una rama local (descarta cambios no fusionados)
git branch -D feature/experimento-obsoleto
```

---

## 5. Resumen de Comandos Principales

| Comando | Operación |
| :--- | :--- |
| `git branch <nombre>` | Crea una rama nueva en la ubicación actual. |
| `git switch <nombre>` | Cambia a la rama especificada. |
| `git switch -c <nombre>` | Crea una rama nueva y cambia a ella de inmediato. |
| `git branch -a` | Muestra todas las ramas (locales y remotas). |
| `git branch -m <nuevo>` | Cambia el nombre de la rama actual. |
| `git branch -d <nombre>` | Elimina una rama local de forma segura. |