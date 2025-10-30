# @arkanjs/baileys

> A modern and optimized WhatsApp Multi-Device library built on top of the Baileys framework — providing enhanced stability, customization, and developer-friendly structure.

---

## 📦 Installation

You can install the package directly from a local folder or npm (once published):

```bash
# Local installation
npm install /path/to/baileys

# Or via npm registry (if published)
npm install @arkanjs/baileys
```

---

## 🚀 Quick Start

Example `index.js` using `@arkanjs/baileys`:

```js
const { makeWASocket, useMultiFileAuthState, DisconnectReason } = require('@arkanjs/baileys');
const P = require('pino');

async function startBot() {
  const { state, saveCreds } = await useMultiFileAuthState('./session');
  const sock = makeWASocket({
    printQRInTerminal: true,
    auth: state,
    logger: P({ level: 'silent' })
  });

  sock.ev.on('creds.update', saveCreds);

  sock.ev.on('connection.update', (update) => {
    const { connection, lastDisconnect } = update;
    if (connection === 'close') {
      const reason = lastDisconnect?.error?.output?.statusCode;
      if (reason !== DisconnectReason.loggedOut) {
        console.log('🔁 Reconnecting...');
        startBot();
      } else {
        console.log('❌ Logged out.');
      }
    } else if (connection === 'open') {
      console.log('✅ Connected to WhatsApp!');
    }
  });
}

startBot();
```

---

## ⚙️ Features

✅ Multi-device WhatsApp connection  
✅ Automatic session saving & reloading  
✅ Reconnect system with error handling  
✅ Simple event system for messages, groups, and status updates  
✅ Easy to extend and modify  

---

## 🧩 Folder Structure

```
@arkanjs/baileys/
├── lib/
│   ├── Socket/
│   │   ├── index.js          # Main connection handler
│   ├── index.js              # Entry point
│   ├── ...
├── package.json
├── README.md
```

---

## 🧠 Customization

You can easily modify behavior by editing files under:

```
/lib/Socket/index.js
/lib/index.js
```

Typical entry points include:
- `makeWASocket()` → creates the main WhatsApp client
- `useMultiFileAuthState()` → handles session storage
- `connection.update` → manages reconnect & QR logic

---

## 🧰 Development

Clone and link locally:

```bash
git clone https://github.com/arkanjs/baileys.git
cd baileys
npm install
npm link
```

Then in your bot project:

```bash
npm link @arkanjs/baileys
```

Now your bot will always use the live, local version of your library.

---

## 🧾 License

This project is licensed under the **MIT License**.  
Feel free to modify and distribute under your own terms.

---

## 👤 Author

**Arkan**  
> Developer • Coder • WhatsApp Automation Enthusiast  
> GitHub: [@arkanjs](https://github.com/arkanjs)

---

## 💬 Support

If you find a bug or need help improving the library, open an issue or contact me directly.  
Contributions, suggestions, and pull requests are always welcome!

---

✨ *Crafted with ❤️ by Arkan — powered by Baileys.*
