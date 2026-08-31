# 04. Resolución de Conflictos

Un conflicto de fusión (*merge conflict*) ocurre cuando Git no puede combinar automáticamente los cambios de dos ramas distintas. Esto sucede comúnmente cuando dos desarrolladores modifican las mismas líneas de un archivo o cuando un archivo es eliminado en una rama pero modificado en otra.

---

## 1. Detección de Conflictos

Cuando ejecutas un `git merge` o `git rebase` y surgen diferencias incompatibles, Git detiene el proceso y te notifica en la terminal:

```bash
Auto-merging src/main.c
CONFLICT (content): Merge conflict in src/main.c
Automatic merge failed; fix conflicts and then commit the result.
```

### Comandos de Diagnóstico

Para ver qué archivos están bloqueando el proceso:

```bash
# Consultar el estado del repositorio
git status

# Ver únicamente la lista de archivos no fusionados (unmerged files)
git diff --name-only --diff-filter=U
```

---

## 2. Anatomía de un Conflicto en el Código

Al abrir un archivo con conflicto en tu editor de texto, verás marcas especiales insertadas por Git:

```c
<<<<<<< HEAD
// Cambios presentes en la rama actual activa (donde estás parado)
uint16_t sensor_read = read_adc_channel(1);
=======
// Cambios provenientes de la rama que estás intentando fusionar
uint16_t sensor_read = read_adc_filtered(1, AVG_SAMPLES);
>>>>>>> feature/filtro-adc
```

### Explicación de las Marcas:
* **`<<<<<<< HEAD`**: Indica el inicio del bloque de código presente en tu rama actual (`HEAD`).
* **`=======`**: Separador entre las dos versiones en conflicto.
* **`>>>>>>> <nombre-de-rama>`**: Indica el final del bloque en conflicto proveniente de la rama que quieres integrar.

---

## 3. Flujo Paso a Paso para Resolver un Conflicto

1. **Abrir el archivo en conflicto.**
2. **Decidir qué código conservar:**
   * Conservar solo los cambios de la rama actual (`HEAD`).
   * Conservar solo los cambios de la rama entrante.
   * Combinar manualmente ambas lógicas de código.
3. **Limpiar las marcas de conflicto:** Elimina las líneas `<<<<<<<`, `=======` y `>>>>>>>`.
4. **Marcar el archivo como resuelto:**
   ```bash
   git add src/main.c
   ```
5. **Completar la fusión:**
   ```bash
   git commit -m "fix: resolver conflicto en lectura de sensor ADC"
   ```

---

## 4. Herramientas y Cancelación

### Abortar un Merge en Curso

Si el conflicto es demasiado complejo o quieres volver al estado exacto previo a la fusión:

```bash
git merge --abort
```

Si estás en medio de un `git rebase` y deseas cancelarlo:

```bash
git rebase --abort
```

### Herramientas Visuales (Mergertool)

Puedes configurar editor de código o herramientas gráficas (como VS Code, Meld, KDiff3) para resolver conflictos de forma visual:

```bash
# Lanzar la herramienta gráfica de resolución de conflictos
git mergetool
```

---

## 5. Buenas Prácticas para Minimizar Conflictos

* **Hacer commits pequeños y frecuentes:** Facilita la identificación de diferencias.
* **Sincronizar la rama principal constantemente:** Ejecuta `git pull` o rebase tus ramas locales con respecto a `main` regularmente.
* **Comunicación en el equipo:** Coordina qué componentes o archivos tocará cada desarrollador.
* **Respetar la modularidad del código:** Separar funcionalidades en archivos independientes reduce la colisión en las mismas líneas de código.