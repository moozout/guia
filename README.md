# Beatrice Bot — Bot de WhatsApp con Baileys

Bot de WhatsApp hecho en Node.js usando [Baileys](https://github.com/WhiskeySockets/Baileys). Incluye descargas (TikTok, YouTube, Facebook, Pinterest), stickers, moderación automática (antilink, antispam, etc.), sistema de perfiles, economía, juegos y más.

100% gratis y sin licencias: no hay comandos de token ni de activación por grupo, todos los grupos pueden usar el bot libremente. Tampoco hay ningún número "owner" fijo en el código — el owner (dueño con todos los permisos) es automáticamente el número de WhatsApp con el que vincules el bot.

> Este bot funciona en tu propio dispositivo Android usando **Termux**. No necesitas un servidor ni VPS.

---

## Requisitos

- Un celular Android (con espacio libre y buena batería/conexión, ya que el bot debe quedarse corriendo).
- La app **Termux**, instalada desde **F-Droid** (**no** desde Play Store, esa versión está desactualizada y no funciona bien).
- Un número de WhatsApp para vincular el bot (recomendado: uno secundario, no tu número principal).

---

## 1. Instalar Termux

Termux **solo** debe instalarse a través de F-Droid. Sigue estos dos pasos en orden:

**Paso 1 — Instalar la app de F-Droid:**

1. Entra a [f-droid.org](https://f-droid.org) y descarga el APK de F-Droid.
2. Instálalo (puede que Android te pida activar "Instalar apps de orígenes desconocidos" — actívalo solo para esta instalación).
3. Abre F-Droid una vez para confirmar que quedó instalado correctamente.

**Paso 2 — Instalar Termux desde F-Droid:**

1. Dentro de la app de F-Droid, o directamente desde este link, entra a: [https://f-droid.org/packages/com.termux/](https://f-droid.org/packages/com.termux/)
2. Toca "Instalar" (o "Download APK" si lo abres desde el navegador y F-Droid no lo detecta automáticamente).
3. Espera a que descargue e instale.
4. Ábrelo. Deberías ver una terminal negra con texto verde/blanco.

---

## 2. Dar acceso al almacenamiento

Para que el bot pueda guardar sesión, descargas temporales, imágenes, etc., dale permiso de almacenamiento a Termux:

```bash
termux-setup-storage
```

Te va a saltar un permiso de Android — **acéptalo**. Esto crea una carpeta `~/storage` con accesos directos a tu almacenamiento (`~/storage/shared`, `~/storage/downloads`, etc.).

---

## 3. Descargar el bot (carpeta `gifs/` + `index.js`)

Con un solo comando, Termux va a instalar `git` y descargar automáticamente el repositorio con `index.js` y la carpeta `gifs/` ya lista, sin que tengas que copiar nada a mano:

```bash
pkg install -y git && git clone https://github.com/moozout/Beatrice-Bot---Files-Guide-and-a-farewell-.git ~/beatrice-bot && cd ~/beatrice-bot
```

Esto va a:
1. Instalar `git` si no lo tienes.
2. Clonar el repositorio dentro de `~/beatrice-bot`.
3. Entrarte automáticamente a esa carpeta.

Al terminar, deberías tener esta estructura sin hacer nada más:

```
beatrice-bot/
├── index.js
└── gifs/
    ├── ship.mp4
    ├── vs.mp4
    ├── mejor.mp4
    ├── rata.mp4
    ├── simp.mp4
    ├── iq.mp4
    ├── gay.mp4
    ├── lesbian.mp4
    ├── bisexual.mp4
    ├── freaky.mp4
    ├── otaku.mp4
    ├── funny.mp4
    └── error.mp4
```

> Puedes confirmarlo corriendo `ls` y luego `ls gifs` — deberías ver el archivo `index.js` y los 13 `.mp4` respectivamente.

---

## 4. Actualizar el bot en el futuro

Si más adelante el repositorio recibe cambios (nuevos gifs, ajustes al `index.js`, etc.), no hace falta borrar todo y volver a clonar. Solo corre:

```bash
cd ~/beatrice-bot
git pull
```

Esto va a traer los cambios más recientes sin tocar tu sesión de WhatsApp ya vinculada (siempre que la carpeta de sesión no esté incluida en el repositorio).

---

## 5. Instalar todo lo necesario (un solo comando)

Parado dentro de la carpeta del bot (`~/beatrice-bot`), corre:

```bash
pkg update -y && pkg upgrade -y && pkg install -y nodejs git ffmpeg python && pip install -U yt-dlp && npm init -y && npm install @whiskeysockets/baileys axios @hapi/boom fluent-ffmpeg node-webpmux form-data
```

Esto instala:
| Herramienta | Para qué sirve |
|---|---|
| `nodejs` | Motor para correr el bot |
| `ffmpeg` | Procesar audio/video/stickers |
| `python` + `yt-dlp` | Descargar videos/audios de YouTube (`#yta`, `#ytv`) |
| `@whiskeysockets/baileys` | Conexión con WhatsApp |
| `axios` | Peticiones HTTP (descargas, QR, etc.) |
| `@hapi/boom` | Manejo de errores de conexión |
| `fluent-ffmpeg` | Wrapper de ffmpeg para Node |
| `node-webpmux` | Metadata (autor/nombre) de los stickers |
| `form-data` | Envío de archivos en peticiones (lector de QR) |

---

## 6. Correr el bot

```bash
node index.js
```

La primera vez te va a pedir vincular tu WhatsApp (escaneando un QR o con un código de emparejamiento). Una vez vinculado, la sesión queda guardada en la carpeta y no tendrás que volver a escanear (a menos que borres esa carpeta o cierres sesión desde el celular).

**Importante:** el número que vincules aquí se convierte automáticamente en el **owner** del bot (acceso total a todos los comandos), así que usa el número que quieras que tenga el control.

### Mantenerlo corriendo sin que se apague la pantalla
```bash
termux-wake-lock
node index.js
```

---

## 7. Reiniciar el bot en el futuro

Cada vez que quieras volver a prenderlo (sin reinstalar nada), solo necesitas:

```bash
cd ~/beatrice-bot
node index.js
```

---

## Solución de problemas

- **"yt-dlp: command not found"**: corre de nuevo `pip install -U yt-dlp`.
- **Error al instalar `@whiskeysockets/baileys`**: prueba `npm install github:WhiskeySockets/Baileys`.
- **El bot se cierra al bloquear pantalla**: usa `termux-wake-lock` antes de `node index.js`, y desactiva la optimización de batería de Termux en Ajustes de Android.
- **No pide permiso de almacenamiento**: ve a Ajustes de Android > Apps > Termux > Permisos > Almacenamiento, actívalo manualmente y vuelve a correr `termux-setup-storage`.
- **Los comandos de Juegos (#ship, #vs, #gay, etc.) no mandan video**: revisa que la carpeta `gifs/` exista junto a `index.js` y que los archivos `.mp4` tengan exactamente esos nombres.

---

## Aviso

Este bot usa una librería no oficial (Baileys) para conectarse a WhatsApp. Usarlo puede ir en contra de los Términos de Servicio de WhatsApp; úsalo bajo tu propio riesgo, de preferencia con un número secundario.

---

## Créditos

Bot creado por **MoozOut**. Basado en [Baileys](https://github.com/WhiskeySockets/Baileys).
