# Eliza 🤖 for Communities

<div align="center">
  <img src="./docs/static/img/eliza_banner.jpg" alt="Eliza Banner" width="60%" />
</div>

<div align="center">
  📖 [Documentation](https://elizaos.github.io/eliza/) | 🎯 [Examples](https://github.com/thejoven/awesome-eliza)
</div>

## 🚩 Overview

<div align="center">
  <img src="./docs/static/img/eliza_diagram.png" alt="Eliza Diagram" width="100%" />
</div>

## ✨ Features

- 🛠️ Full-featured connectors for Discord, Twitter, and Telegram
- 🔗 Supports various models (Llama, Grok, OpenAI, Anthropic, etc.)
- 👥 Multi-agent and room support
- 📚 Easy document ingestion and interaction
- 💾 Retrievable memory and document store
- 🚀 High extensibility for custom actions and clients
- ☁️ Compatible with many models (local Llama, OpenAI, Anthropic, Groq, etc.)
- 📦 Ready to use!

## Important Notice

This guide is designed to help communities run their own bots effortlessly.

### Prerequisites

- [Python 2.7+](https://www.python.org/downloads/)
- [Node.js 23+](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
- Use `nvm` to set Node.js version: `nvm use v23.3.0` or `nvm alias default v23.3.0`
- [pnpm](https://pnpm.io/installation)

### Creating the File Structure

```bash
git clone https://github.com/elizaos/eliza.git
```

### First Build Initialization

1. Install dependencies and build:

   ```bash
   pnpm i
   pnpm build
      ```

2. To keep the instance running after logout, use `pm2`:

   ```bash
   npm install pm2
   ```

3. Start the process with `pm2`:

   ```bash
   pm2 start pnpm --name eliza -- start -- --character=./eliza/characters/avatar.character.json
   ```

   - **PM2 Commands:**
     - `pm2 start eliza`
     - `pm2 stop eliza`
     - `pm2 log`
     - `pm2 status`
     - `pm2 save`


### Edit the `.env` File

The `.env` file contains all API keys, seed phrases, and credentials for Twitter, Discord, and Telegram. Do not share this file.
Best to use is VS Code, but you can also use any text-based editor or even the commandline editor

1. Copy the example file:

   ```bash
   cp .env.example .env
   ```

2. Edit the file using `nano`:

   ```bash
   nano .env
   ```

   - **Nano Commands:**
     - `CTRL+X`: Save and Exit
     - `CTRL+W`: Search

### Obtain  API Key



1. Edit the `.env` file to add your API key:

   ```bash
   nano .env
   ```
Insert your API KEY into the according line
  

### Edit Your Character File

1. Navigate to the character folder:

   ```bash
   cd characters
   ```

2. Use `c3po.character.json` as a template:

   ```bash
   cp c3po.character.json avatar.character.json
   nano avatar.character.json
   ```

   - **Structure:**
     - **Bio:** Self-definition for the LLM
     - **Lore:** Text style examples
     - **Knowledge:** Initial memories (e.g., website, Discord URL)

3. Set the attribute "modelProvider" to use the AKASH Chat API :

   ```json
   "modelProvider": "akash_chat_api",
   ```




### First Chat
Start the framework

```bash
pnpm start --characters="./eliza/characters/avatar.character.json"
```bash

And open a new window and run:

```bash
pnpm start:client
```

Visit the displayed website on your localhost using a web browser.

### Telegram Connection

1. Use [BotFather](https://telegram.me/BotFather) to create a bot.
2. Add your bot token to the `.env` file.
3. Update your character file header:

   ```json
   "clients": ["telegram"],
   ```

   Ensure a space after the colon: `"clients": ["telegram"],`.

### Discord Connection

1. Create a bot token at [Discord Developers](https://discord.com/developers).
2. Reset your token if necessary.
3. Use the invite link from startup to add the bot to your server.
4. Set bot permission intentions to the right.
5. Enter your Application ID and API Token as shown.

## Advanced Character Settings

Example:

```json
{
  "name": "Bud",
  "plugins": ["web-search"],
  "clients": ["discord"],
  "modelProvider": "akash_chat_api",
  "settings": {
    "secrets": {
      "DISCORD_APPLICATION_ID": "123456789",
      "DISCORD_API_TOKEN": "MTMzMDEzMzE4NXXXXXXXXXXXXXXXK3zQp577QWKjS4i-wz78"
    },
    "voice": {
      "model": "en_US-male-medium"
    }
  },
```

- **Secrets:** Override values in `.env` for a specific character.
- **Plugins:** Embed plugins from the `/packages` folder.

### Accessing the Database

```bash
cd /agent/data
```

Find the `db.sqlite` file and use `sqlitebrowser` to access it:

```bash
sqlitebrowser db.sqlite
```


## Contributors

<a href="https://github.com/elizaos/eliza/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=elizaos/eliza" />
</a>

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=elizaos/eliza&type=Date)](https://star-history.com/#elizaos/eliza&Date)
