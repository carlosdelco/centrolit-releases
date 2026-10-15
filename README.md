# APP de Escritorio Centrolit — Cotizador y Mando (instaladores)

Descargas de la app de escritorio del Grupo Centrolit para **Windows**.

1. Ve a **[Releases](../../releases/latest)** y descarga `Centrolit-Setup-x.y.z.exe`.
2. Ejecútalo. Como el instalador todavía no está firmado, Windows puede mostrar un aviso de SmartScreen:
   pulsa **Más información → Ejecutar de todos modos**.
3. Abre la app e inicia sesión con tu cuenta de Google del Grupo.

La app se actualiza sola: al abrirla comprueba si hay una versión nueva, la descarga y te pide reiniciar.
Los precios y KPIs se leen de Google Sheets y se refrescan al abrir la app o con el botón **Refrescar**
(no dependen de la versión instalada).

Este repositorio solo contiene los instaladores. Sin una cuenta de Google con permiso sobre los ficheros
del Grupo, la app solo muestra datos de demostración.

# Web App
Publicada en: https://carlosdelco.github.io/centrolit-web/ (GitHub Pages, repo público centrolit-web — solo el build estático, sin código fuente ni datos reales). Sin GOOGLE_CLIENT_ID_WEB configurado, solo funciona el modo demo.

npm run build:web    # ensambla web-dist/ (carpeta estática autocontenida)
npm run serve:web    # la sirve en local, http://localhost:8080
npm run deploy:web   # construye y publica en https://carlosdelco.github.io/centrolit-web/

Para usarla con datos reales hace falta un cliente OAuth de tipo Aplicación web (distinto al de escritorio): ver docs/SETUP-GOOGLE.md > sección D.

Cómo funciona por dentro: sin servidor propio. El navegador habla directo con Google (Google Identity Services para el login, Sheets API para los datos). No hay backend que guarde nada: ni tokens, ni client secret —de hecho esta versión no usa client secret en absoluto, solo un Client ID público—, ni datos del Grupo. src/renderer/cotizador.js se reutiliza tal cual en ambas versiones: detecta en tiempo de ejecución si existe window.centrolit (escritorio) y, si no, usa el camino nativo del navegador para exportar (window.print() y una descarga de Blob), igual que hacía el cotizador original. Ver docs/SEGURIDAD.md para las diferencias de seguridad frente al escritorio.
