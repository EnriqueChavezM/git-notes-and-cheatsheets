# 03. Gestión de Cambios y Deshacer Acciones

Esta guía abarca las técnicas y comandos necesarios para comparar diferencias, revertir modificaciones no deseadas, restaurar el estado de los archivos y modificar confirmaciones (commits) en Git.

---

## 1. Comparación de Cambios (`git diff`)

Muestra las diferencias de código entre distintas fases del repositorio.

```bash
# Comparar el directorio de trabajo actual con el Área de Preparación (Staging Area)
git diff

# Comparar los cambios guardados en Staging Area con el último commit (HEAD)
git diff --staged

# Comparar los cambios entre dos commits específicos mediante sus hashes
git diff hash_antiguo hash_nuevo

# Ver únicamente los archivos que han cambiado (sin mostrar el código)
git diff --stat
```

---

## 2. Modificación del Último Commit (`git commit --amend`)

Permite agregar cambios olvidados o corregir el mensaje del último commit realizado sin generar una nueva entrada en el historial.

```bash
# Cambiar el mensaje del último commit realizado
git commit --amend -m "docs: corregir descripción del módulo principal"

# Agregar un archivo olvidado al último commit (sin abrir el editor de texto)
git add archivo_olvidado.py
git commit --amend --no-edit
```
> **Nota de seguridad:** Evita usar `--amend` en commits que ya hayan sido subidos (*pushed*) a un repositorio remoto.

---

## 3. Descartar Cambios Locales (`git checkout` / `git restore`)

### Restaurar Archivos en el Directorio de Trabajo

Descarta las modificaciones locales de un archivo no guardado en el área de preparación, regresándolo al estado del último commit.

```bash
# Usando el comando moderno (recomendado)
git restore archivo.txt

# Descartar todos los cambios no guardados en el directorio actual
git restore .

# Usando el comando tradicional
git checkout -- archivo.txt
```

### Sacar Archivos del Área de Preparación (*Unstaging*)

Quita un archivo del área de preparación manteniendo las modificaciones en tu directorio de trabajo.

```bash
# Usando el comando moderno (recomendado)
git restore --staged archivo.txt

# Usando el comando tradicional
git reset HEAD archivo.txt
```

---

## 4. Reversión e Historial (`git reset` vs `git revert`)

### `git reset` (Reescribe el historial local)
Mueve el puntero de la rama actual a un commit anterior. 

```bash
# Reset Soft: Mantiene los cambios en el área de preparación (Staging)
git reset --soft HEAD~1

# Reset Mixed (por defecto): Mantiene los cambios en el directorio de trabajo pero los saca de Staging
git reset --mixed HEAD~1

# Reset Hard: Elimina de forma destructiva todos los cambios realizados después del commit indicado
git reset --hard HEAD~1
```

### `git revert` (Reversión segura en remoto)
Crea un **nuevo commit** que deshace exactamente los cambios de una confirmación anterior. Es la opción recomendada para proyectos compartidos.

```bash
# Revertir un commit específico por su hash
git revert hash_del_commit

# Revertir sin abrir el editor de mensajes para hacer commit automático
git revert hash_del_commit --no-edit
```

---

## 5. Resumen de Comandos

| Comando | Operación | Efecto en el Historial |
| :--- | :--- | :--- |
| `git diff` | Muestra diferencias de código. | Ninguno (sólo lectura). |
| `git restore <archivo>` | Descarta cambios locales sin guardar. | Ninguno (no modifica commits). |
| `git restore --staged <archivo>` | Saca el archivo del área de *staging*. | Ninguno. |
| `git commit --amend` | Reescribe la confirmación anterior. | Modifica el último commit. |
| `git reset --hard <hash>` | Regresa el proyecto al estado exacto del hash. | Destructivo (elimina commits posteriores). |
| `git revert <hash>` | Deshace los cambios de un commit mediante otro nuevo. | Conservador (añade un nuevo commit). |