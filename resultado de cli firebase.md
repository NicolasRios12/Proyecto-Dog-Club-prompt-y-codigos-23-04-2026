Para trabajar con Firebase en Flutter desde Windows, es fundamental configurar correctamente el entorno de Node.js, ya que la herramienta de línea de comandos (CLI) de Firebase depende de este ecosistema.

Aquí tienes la guía paso a paso para configurar tu entorno de desarrollo.

---

### 1. Software necesario: Node.js y npm
Firebase CLI funciona como un paquete de Node.js. Al instalar Node.js, obtienes automáticamente **npm** (Node Package Manager), que es la herramienta que usaremos para instalar Firebase.

#### ¿Cómo verificar si ya lo tienes instalado?
1. Abre la terminal (PowerShell o CMD).
2. Escribe los siguientes comandos uno por uno y presiona Enter:
   ```bash
   node -v
   npm -v
   ```
3. **Resultado:** Si ves un número de versión (ej. `v20.x.x`), ya lo tienes instalado y puedes saltar al paso 2. Si recibes un error como *"no se reconoce como un comando"*, procede con la instalación.

#### Paso a paso: Instalación de Node.js en Windows
1. Ve al [sitio oficial de Node.js](https://nodejs.org/).
2. Descarga la versión **LTS (Long Term Support)**; es la más estable para desarrollo.
3. Ejecuta el instalador (`.msi`) que descargaste.
4. Sigue el asistente de instalación (puedes dejar todas las opciones predeterminadas).
   * *Nota:* Asegúrate de que la opción "Add to PATH" esté seleccionada (suele estar marcada por defecto).
5. Al finalizar, **cierra y vuelve a abrir tu terminal** para que los cambios en las variables de entorno surtan efecto.
6. Vuelve a ejecutar `node -v` y `npm -v` para confirmar que la instalación fue exitosa.

---

### 2. Instalación de Firebase CLI
Una vez que `npm` funciona, puedes instalar las herramientas de Firebase de manera global en tu sistema.

1. En tu terminal, ejecuta el siguiente comando:
   ```bash
   npm install -g firebase-tools
   ```
   * El indicador `-g` le dice a `npm` que instale la herramienta **globalmente**, permitiéndote usar el comando `firebase` desde cualquier carpeta de tu computadora.

2. Verifica que la instalación fue correcta escribiendo:
   ```bash
   firebase --version
   ```

---

### 3. Acceso a Firebase con tu cuenta de Google
Para que la CLI pueda comunicarse con tus proyectos en la consola de Firebase, debes autenticarte.

1. Ejecuta el comando:
   ```bash
   firebase login
   ```
2. **Qué sucede:**
   * La terminal te preguntará si permites la recopilación de errores (es opcional, puedes poner `Y` o `N`).
   * Se abrirá automáticamente una ventana en tu navegador web predeterminado.
   * Selecciona la **Cuenta de Google** que utilizas para tu proyecto de Firebase.
   * Haz clic en **Permitir** cuando se te solicite acceso.
3. Una vez autenticado, verás un mensaje de éxito en tu navegador y la terminal confirmará que has iniciado sesión con tu correo electrónico.

---

### 4. Extra: Configuración específica para Flutter (FlutterFire)
Como estás trabajando específicamente con Flutter, necesitarás una herramienta adicional llamada **FlutterFire CLI**. Esta herramienta automatiza la configuración de Firebase dentro de tu proyecto Flutter.

* **Instalación:**
  ```bash
  dart pub global activate flutterfire_cli
  ```
* **Uso:** Esta herramienta se ejecuta dentro de la carpeta raíz de tu proyecto Flutter usando:
  ```bash
  flutterfire configure
  ```

---

¿Has logrado realizar la autenticación exitosamente en tu navegador o te ha surgido algún problema al intentar ejecutar `firebase login`?
