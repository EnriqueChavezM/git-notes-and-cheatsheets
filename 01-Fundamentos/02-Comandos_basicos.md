# 02. Comandos Básicos de Git

Esta guía cubre el ciclo de vida fundamental para rastrear y documentar cambios en un proyecto utilizando Git: desde la inicialización del repositorio hasta la creación de confirmaciones (commits) y la inspección del historial.

---

## 1. Inicialización y Estado del Repositorio

### `git init`
Inicializa un nuevo repositorio Git en el directorio actual creando la carpeta oculta `.git`.

```bash
git init
```

### `git status`
Muestra el estado actual del árbol de trabajo y del área de preparación (staging area). Indica qué archivos han sido modificados, cuáles están rastreados y cuáles no.

```bash
git status
```
* **Tip:** Usa `git status -s` para obtener una vista resumida del estado.

---

## 2. Área de Preparación (Staging Area)

El área de preparación permite seleccionar exactamente qué cambios incluir en el próximo commit.

### `git add`
Agrega archivos modificados o nuevos al área de preparación.

```bash
# Agregar un archivo específico
git add archivo.txt

# Agregar múltiples archivos especificados
git add archivo1.txt archivo2.py

# Agregar todos los archivos modificados y nuevos del directorio actual
git add .
```

---

## 3. Confirmación de Cambios (Commits)

Un *commit* guarda un foto fija (snapshot) de los archivos que se encuentran en el área de preparación.

### `git commit`
Registra los cambios en el historial del repositorio acompañados de un mensaje descriptivo.

```bash
# Commit con mensaje en línea
git commit -m "feat: agregar módulo de configuración inicial"

# Agregar cambios rastreados y hacer commit en un solo paso de archivo que ya esta bajo el  control de versión
git commit -a -m "fix: corregir error de sintaxis en script principal"
```

---

## 4. Inspección del Historial

### `git log`
Muestra la lista de commits realizados en la rama actual en orden cronológico inverso.

```bash
# Historial completo detallado
git log

# Historial resumido en una sola línea por commit
git log --oneline

# Historial gráfico con ramas y etiquetas
git log --oneline --graph --all
```

---

## 5. Resumen del Flujo de Trabajo Básico

| Comando | Acción / Propósito |
| :--- | :--- |
| `git init` | Crea un repositorio local desde cero. |
| `git status` | Consulta qué archivos tienen cambios pendientes. |
| `git add <archivo>` | Pasa cambios de la zona de trabajo a la zona de *staging*. |
| `git commit -m "mensaje"` | Registra permanentemente los cambios guardados en *staging*. |
| `git log --oneline` | Muestra un resumen condensado de las confirmaciones. |