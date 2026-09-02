# 01. Rebase Interactivo (Interactive Rebase)

El rebase interactivo (`git rebase -i`) es una de las herramientas más potentes de Git para pulir, organizar y reestructurar el historial de commits local antes de integrarlo en ramas compartidas o enviarlo a un repositorio remoto. Permite modificar commits pasados de forma selectiva.

---

## 1. ¿Cuándo usar el Rebase Interactivo?

* **Limpiar el historial:** Unir commits temporales (ej. *"WIP"*, *"fix menor"*, *"pruebas"*) en un único commit coherente.
* **Corregir mensajes:** Modificar errores tipográficos o ajustar mensajes de commit a las convenciones del proyecto (Conventional Commits).
* **Dividir o modificar commits:** Agregar o eliminar cambios específicos realizados en un commit anterior.
* **Reordenar o eliminar:** Cambiar la secuencia lógica de confirmaciones o eliminar por completo un commit innecesario.

---

## 2. Iniciar una Sesión de Rebase Interactivo

Para iniciar la reestructuración interactiva, debes indicar a Git hasta qué punto del historial deseas retroceder:

```bash
# Modificar los últimos N commits a partir de HEAD
git rebase -i HEAD~4

# Modificar commits desde una rama base (ej. main)
git rebase -i main

# Modificar commits a partir de un Hash específico (no incluido)
git rebase -i 3f5a2b1
```

Al ejecutar el comando, Git abrirá tu editor de texto predeterminado con la lista de commits en orden **cronológico inverso** (del más antiguo al más reciente) y un menú de instrucciones.

---

## 3. Comandos del Editor Interactivo

Cada línea de la lista comienza con una palabra clave que indica la acción que Git ejecutará sobre ese commit:

| Comando | abreviatura | Descripción |
| :--- | :---: | :--- |
| **`pick`** | `p` | Utiliza el commit tal cual (opción por defecto). |
| **`reword`** | `r` | Utiliza el commit, pero abre el editor para cambiar el mensaje. |
| **`edit`** | `e` | Detiene el rebase en ese commit para modificar archivos o agregar/quitar cambios. |
| **`squash`** | `s` | Combina el commit con el anterior y fusiona ambos mensajes en uno solo. |
| **`fixup`** | `f` | Combina el commit con el anterior descartando el mensaje del commit actual. |
| **`exec`** | `x` | Ejecuta un comando de la terminal (ej. pruebas automáticas) en esa etapa. |
| **`drop`** | `d` | Elimina por completo el commit del historial. |

---

## 4. Casos Prácticos Comunes

### Caso 1: Combinar Commits Pequeños (`squash` / `fixup`)

Si realizaste varios commits de prueba mientras desarrollabas una característica:

```text
pick a1b2c3d feat: agregar controlador SPI
pick e4f5g6h fix: corregir baudrate en SPI
pick i7j8k9l typo: corregir comentario en driver
```

Puedes unificarlos cambiando las acciones a `fixup` (o `f`):

```text
pick a1b2c3d feat: agregar controlador SPI
fixup e4f5g6h fix: corregir baudrate en SPI
fixup i7j8k9l typo: corregir comentario en driver
```
*Resultado:* Los tres commits se integran en uno solo con el mensaje `"feat: agregar controlador SPI"`.

---

### Caso 2: Editar el Código de un Commit Anterior (`edit`)

1. En la lista interactiva, cambia `pick` por `edit` (o `e`) en el commit deseado.
2. Guarda y cierra el editor. Git pausará el proceso en ese commit exacto.
3. Modifica los archivos en tu editor de código.
4. Prepara y enmienda el commit:
   ```bash
   git add src/driver_spi.c
   git commit --amend --no-edit
   ```
5. Continúa con la secuencia de rebase:
   ```bash
   git rebase --continue
   ```

---

## 5. Control de Errores y Cancelación

Si surgen conflictos complejos o cometes un error durante la edición del rebase interactivo, puedes cancelar el proceso completamente y devolver la rama a su estado original sin sufrir pérdida de datos:

```bash
git rebase --abort
```