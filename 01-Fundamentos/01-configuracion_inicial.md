# 01. Configuración Inicial de Git y GitHub

Esta guía abarca los pasos iniciales para instalar Git, configurar tu identidad local y vincular tu cuenta con GitHub de manera segura.

---

## 1. Verificación e Instalación

Antes de comenzar, verifica si ya tienes Git instalado en tu sistema ejecutando en la terminal:

```bash
git --version
```

### Comandos de instalación según el sistema operativo:

* **Linux (Ubuntu/Debian):**
  ```bash
  sudo apt update
  sudo apt install git-all
  ```
* **macOS (vía Homebrew):**
  ```bash
  brew install git
  ```
* **Windows:**
  Descarga el instalador oficial desde [git-scm.com](https://git-scm.com/) o ejecuta en PowerShell:
  ```powershell
  winget install --id Git.Git -e --source winget
  ```

---

## 2. Configuración de Identidad (Global)

Configura el nombre y correo electrónico que firmarán tus commits. **Usa el mismo correo asociado a tu cuenta de GitHub** para que tus contribuciones se vinculen correctamente a tu perfil.

```bash
# Define tu nombre público en los commits
git config --global user.name "NOMBRE_USUARIO"

# Define tu correo electrónico registrado en GitHub
git config --global user.email "EMAIL_USUARIO@ejemplo.com"
```

---

## 3. Configuraciones Esenciales Recomendadas

Ajustes iniciales para estandarizar el flujo de trabajo y evitar problemas de compatibilidad:

```bash
# Definir 'main' como la rama por defecto al inicializar repositorios
git config --global init.defaultBranch main

# Configurar el manejo de saltos de línea (Windows: true | Mac/Linux: input)
git config --global core.autocrlf true

# Editor de texto por defecto para mensajes de commit (ejemplo: VS Code)
git config --global core.editor "code --wait"

# Formato de salida de colores en la terminal para mejor legibilidad
git config --global color.ui auto
```

---

## 4. Verificación de la Configuración

Para listar todos los parámetros configurados en tu entorno y ver de qué archivo provienen:

```bash
git config --list --show-origin
```

O para consultar un parámetro específico:

```bash
git config user.name
git config user.email
```

---

## 5. Autenticación Segura con GitHub (SSH)

GitHub ya no permite autenticación mediante contraseña para operaciones Git por terminal. Se recomienda configurar una clave SSH.

### Pasos para generar y añadir tu clave SSH:

1. **Generar la clave SSH:**
   ```bash
   ssh-keygen -t ed25519 -C "tu_email@ejemplo.com"
   ```
   *(Presiona `Enter` para aceptar la ubicación por defecto y define una frase de paso opcional).*

2. **Iniciar el agente SSH y añadir la clave:**
   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   ```

3. **Copiar la clave pública:**
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
   *(Copia todo el contenido mostrado en la terminal).*

4. **Agregar la clave en GitHub:**
   - Ve a **GitHub.com** > **Settings** > **SSH and GPG keys**.
   - Haz clic en **New SSH key**, asigna un título (ej. `Laptop-Personal`) y pega la clave pública.

5. **Probar la conexión:**
   ```bash
   ssh -T git@github.com
   ```
   *(Si el proceso fue exitoso, verás un mensaje de bienvenida de GitHub).*

---

## 6. Resumen de Comandos Útiles

| Comando | Descripción |
| :--- | :--- |
| `git config --help` | Abre la documentación oficial del comando config. |
| `git config --global --list` | Muestra la lista completa de configuraciones globales. | Escribe Q para salir
| `git config --global --unset <clave>` | Elimina un parámetro configurado globalmente. |
| `git config --global alias.<alias> <comando>` | Crea un atajo personalizado para un comando Git. |