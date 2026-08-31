# 03. Merge vs. Rebase

Tanto `git merge` como `git rebase` son comandos diseñados para integrar cambios de una rama en otra. Sin embargo, lo hacen de formas fundamentalmente distintas, impactando la estructura y linealidad del historial del repositorio.

---

## 1. Fusión de Ramas (`git merge`)

`git merge` toma los cambios de la rama de origen y los combina con la rama de destino mediante un nuevo commit especial llamado **Merge Commit**.

### Características:
* **Preserva la historia real:** Mantiene intacto el historial cronológico exacto de cómo y cuándo se desarrollaron los commits en cada rama.
* **No destructivo:** No altera ni reescribe los commits existentes.
* **Crea Merge Commits:** Genera un commit extra con dos padres que une ambas líneas de tiempo.

### Tipos de Merge:
1. **Fast-Forward Merge:** Ocurre cuando no hay commits nuevos en la rama destino. Git simplemente mueve el puntero hacia adelante sin crear un commit de fusión.
2. **3-Way Merge:** Ocurre cuando ambas ramas han avanzado con commits independientes. Git realiza una comparación de tres vías (ambas puntas y el ancestro común) y genera un **Merge Commit**.

```bash
# Integrar la rama feature dentro de main usando merge
git switch main
git merge feature/login
```

---

## 2. Reorganización del Historial (`git rebase`)

`git rebase` toma todos los commits realizados en la rama actual y los "vuelve a aplicar" uno a uno sobre la punta de otra rama destino.

### Características:
* **Historial lineal:** Elimina los commits de fusión innecesarios, creando una secuencia de historial limpia y directa.
* **Reescribe la historia:** Modifica los *hashes* e identidades de los commits originales al crear copias de los mismos en la nueva base.
* **Facilita la lectura:** Permite revisar el historial con `git log --oneline` de forma más ordenada.

```bash
# Rebasar la rama feature sobre main
git switch feature/login
git rebase main

# Después de rebasar, la fusión en main será Fast-Forward
git switch main
git merge feature/login
```

---

## 3. Rebase Interactivo (`git rebase -i`)

Permite modificar, combinar, renombrar o eliminar commits de tu rama local antes de integrarla o subirla a un remoto.

```bash
# Iniciar rebase interactivo para los últimos 3 commits
git rebase -i HEAD~3
```

### Comandos Comunes en el Editor Interactivo:
* **`pick` (p):** Conserva el commit tal como está.
* **`reword` (r):** Usa el commit pero permite modificar el mensaje de confirmación.
* **`squash` (s):** Combina el commit con el commit anterior y fusiona sus mensajes.
* **`fixup` (f):** Combina el commit con el anterior descartando el mensaje de este último.
* **`drop` (d):** Elimina el commit del historial.

---

## 4. La Regla de Oro del Rebase

> **NUNCA utilices `git rebase` en ramas públicas o compartidas con otros desarrolladores.**

Si rebasas una rama que otros compañeros ya han descargado, reescribirás el historial remoto. Esto causará conflictos severos, duplicación de commits y pérdida de código para el resto del equipo.

* **Usa `rebase`:** En tus ramas locales de trabajo personal (*feature branches*) para mantenerte al día con `main` antes de abrir un Pull Request.
* **Usa `merge`:** En ramas públicas (`main`, `develop`) o al integrar Pull Requests finalizados.

---

## 5. Cuadro Comparativo

| Aspecto | `git merge` | `git rebase` |
| :--- | :--- | :--- |
| **Estructura del Historial** | Ramificado y complejo (refleja la realidad). | Lineal, limpio y directo. |
| **Commit de Fusión** | Sí (salvo en casos Fast-Forward). | No. |
| **Reescritura de commits** | No altera el historial existente. | Sí, genera nuevos hashes para los commits. |
| **Manejo de Conflictos** | Se resuelven de una sola vez durante el merge. | Se resuelven commit por commit durante el rebase. |
| **Seguridad** | Altamente seguro en cualquier entorno. | Requiere precaución (solo ramas locales/privadas). |