# Telegram Audio Tasks

Proyecto de automatización que convierte audios enviados por Telegram en tareas organizadas dentro de Trello.

El flujo utiliza **Telegram**, **Make**, **Cloudflare Workers AI** y **Trello**, y está pensado para funcionar con los planes gratuitos de cada servicio, siempre que se mantenga un uso moderado y dentro de sus límites.

## ¿Cómo funciona?

```text
Telegram → Make → Cloudflare Worker → Trello
```

1. El usuario envía una nota de voz al bot de Telegram.
2. Make descarga el audio y lo envía al Cloudflare Worker.
3. Workers AI transcribe el audio y extrae las tareas mencionadas.
4. Make recibe las tareas y crea las tarjetas correspondientes en Trello.

## Archivos

```text
blueprint-public.json
worker.js
```

* `blueprint-make.json`: flujo que se puede importar en Make.
* `Cloudflare WOrker.js`: código que debe copiarse en un Cloudflare Worker.

## Configuración

### 1. Crear el bot de Telegram

Crea un bot desde Telegram utilizando **@BotFather** y guarda el token generado.

Después, conecta ese bot al módulo **Telegram — Watch Updates** dentro de Make. Al establecer la conexión, Make configura el webhook necesario para recibir los mensajes.

### 2. Crear el Cloudflare Worker

Crea un nuevo Worker en Cloudflare y pega el contenido de `worker.js`.

También debes:

* Activar el binding de Workers AI con el nombre `AI`.
* Crear una variable secreta llamada `WORKER_SECRET`.
* Publicar el Worker y copiar su URL.

### 3. Importar el flujo en Make

En Make:

1. Crea un escenario nuevo.
2. Selecciona **Import Blueprint**.
3. Importa `blueprint-make.json`.
4. Conecta tu propio bot de Telegram.
5. Coloca la URL de tu Worker.
6. Agrega el encabezado `x-api-key` con tu propio `WORKER_SECRET`.
7. Conecta tu propia cuenta de Trello.
8. Selecciona el tablero y la lista donde se crearán las tareas.

## Seguridad

Este repositorio no incluye claves, tokens, webhooks, conexiones ni identificadores personales.

**Cada usuario debe configurar sus propias cuentas y credenciales.**
