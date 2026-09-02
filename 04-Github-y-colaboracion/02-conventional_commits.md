# 02. Conventional Commits

**Conventional Commits** es una especificación ligera para añadir significado legible tanto para humanos como para máquinas a los mensajes de confirmación. Facilita la generación automática de historiales de cambio (*CHANGELOG*), el etiquetado semántico de versiones (SemVer) y la auditoría del proyecto.

---

## 1. Estructura del Mensaje

```text
<tipo>[ámbito opcional]: <descripción corta>

[cuerpo opcional]

[pie de página opcional]
```

### Ejemplo Completo:
```text
feat(adc): añadir filtrado por media móvil

Implementa un filtro digital de media móvil de 8 muestras para suavizar
las lecturas analógicas de la entrada A0.

Closes #104
```

---

## 2. Tipos Principales (`<tipo>`)

| Tipo | Descripción | Ejemplo |
| :--- | :--- | :--- |
| **`feat`** | Una nueva funcionalidad para el usuario/sistema. | `feat(ui): agregar modo oscuro` |
| **`fix`** | Solución a un error o bug. | `fix(spi): corregir desbordamiento de buffer` |
| **`docs`** | Cambios únicamente en la documentación. | `docs: actualizar instrucciones de instalación` |
| **`style`** | Formateo de código sin cambio de lógica (espacios, comas, etc.). | `style: aplicar formato clang-format` |
| **`refactor`** | Reestructuración de código que no corrige bug ni añade feature. | `refactor: simplificar bucle de control` |
| **`test`** | Añadir o corregir pruebas unitarias/integración. | `test: agregar test unitario para checksum` |
| **`chore`** | Tareas de mantenimiento, dependencias o compilación. | `chore: actualizar versión de compilador` |

---

## 3. Cambios Incompatibles (*Breaking Changes*)

Para notificar modificaciones que rompen la compatibilidad con versiones anteriores:

1. **Añadir `!` tras el tipo/ámbito:**
   ```text
   refactor(api)!: cambiar la firma de la función inicializadora
   ```
2. **Incluir `BREAKING CHANGE:` en el pie del mensaje:**
   ```text
   feat(i2c): cambiar dirección por defecto del sensor

   BREAKING CHANGE: La dirección I2C por defecto pasa de 0x68 a 0x69.
   ```

---

## 4. Beneficios del Estándar

* **Generación Automática de CHANGELOGs:** Herramientas como `standard-version` pueden leer el historial y crear documentos de lanzamiento automáticamente.
* **Control de Versionado Semántico (SemVer):**
  * `fix` $\rightarrow$ Incrementa versión **PATCH** (`v1.0.0` a `v1.0.1`).
  * `feat` $\rightarrow$ Incrementa versión **MINOR** (`v1.0.0` a `v1.1.0`).
  * `BREAKING CHANGE` $\rightarrow$ Incrementa versión **MAJOR** (`v1.0.0` a `v2.0.0`).