# 01. Pull Requests y Code Reviews

Un **Pull Request (PR)** o **Merge Request** es la propuesta formal de cambios realizada en una rama secundaria para ser integrada en la rama principal del repositorio (`main` o `develop`). Es el corazón del trabajo colaborativo en plataformas como GitHub o GitLab.

---

## 1. Anatomía de un Pull Request

Un PR efectivo debe ser pequeño, con un propósito claro y fácil de revisar por otros desarrolladores:

* **Título Claro:** Resumen directo del cambio (ej. `feat(spi): implementar lectura por interrupciones`).
* **Descripción:** 
  * ¿Qué problema resuelve o qué función añade?
  * ¿Cómo se implementó?
  * Instrucciones paso a paso para probar los cambios.
* **Contexto/Issues Vinculados:** Referencias a tarjetas de trabajo o tareas (`Closes #42`).
* **Capturas de pantalla o logs:** Evidencia visual o registros de compilación/salida si aplica.

---

## 2. Flujo de Trabajo para Crear un PR

1. **Sincronizar y crear rama:**
   ```bash
   git switch main
   git pull origin main
   git switch -c feature/nuevo-sensor
   ```
2. **Desarrollar y subir cambios:**
   ```bash
   git add .
   git commit -m "feat: agregar soporte para sensor BMP280"
   git push origin feature/nuevo-sensor
   ```
3. **Abrir la solicitud:** En la interfaz de GitHub, seleccionar la rama base (`main`) y la rama de origen (`feature/nuevo-sensor`).

---

## 3. Buenas Prácticas en Revisiones de Código (Code Reviews)

### Para el Autor:
* **PRs Pequeños:** Limita los cambios a menos de 300-400 líneas para acelerar la revisión y reducir el sesgo de fatiga.
* **Autorrevisión:** Revisa tu propio *diff* en la plataforma antes de asignar revisores.
* **Pruebas Verificadas:** Asegúrate de que las pruebas automatizadas (CI/CD) pasen en verde.

### Para el Revisor:
* **Enfoque constructivo:** Ofrece sugerencias amables con ejemplos concretos de código.
* **Priorizar:** Distingue entre bloqueos críticos (bugs, fallos de seguridad) y preferencias personales (*nitpicks*).
* **Verificar el fondo:** Evalúa la arquitectura, legibilidad, manejo de errores y rendimiento, no solo el formato del texto.

---

## 4. Estrategias de Fusión (Merge Options)

| Método | Funcionamiento | Resultado en el Historial |
| :--- | :--- | :--- |
| **Create a Merge Commit** | Une ambas ramas creando un commit con dos padres. | Preserva el historial completo de la rama. |
| **Squash and Merge** | Combina todos los commits del PR en uno solo en `main`. | Mantiene un historial limpio y compacto en la rama principal. |
| **Rebase and Merge** | Rebasea los commits individuales del PR sobre `main`. | Mantiene una secuencia lineal sin commits de fusión. |