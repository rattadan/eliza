# Eliza 🤖 on Cosmos Interchain - Using AKASH Powered Resources

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

This guide is designed to help communities run their own bots effortlessly. However, it can be misused by scammers. Please share this guide only with trusted teams, developers, communities, and builders.

Scammers harm the ecosystem by driving people away. To scammers: you won't succeed. Get a real life and stop exploiting others. Talented individuals don't need to scam. It's time to reflect and change.

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

### Obtain Akash API Key

Visit [Akash API](https://chatapi.akash.network/) to generate an API key. The service is free but rate-limited. For extensive use, consider setting up a dedicated environment.

1. Edit the `.env` file to add your API key:

   ```bash
   nano .env
   ```

   Search for "AKASH" using `CTRL+W` and enter your key:

   ```bash
   AKASH_CHAT_API_KEY=sk-1agDViherhwqzr3r # REPLACE YOUR KEY HERE
   ```


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

Check the "memory" table.

## Advanced Vector Indexing Database

For big workloads and long-trained models, consider upscaling your database.

Deploy an AKASH Postgres Vector-enabled Database using this YAML sheet:

```yml
---
version: "2.0"

services:
  postgres:
    image: ankane/pgvector
    expose:
      - port: 5432
        to:
          - global: true
    env:
      - PGDATA=/var/lib/postgresql/data/pgdata
      - "POSTGRES_USER=admin"
      - "POSTGRES_PASSWORD=let-me-in"
      - "POSTGRES_DB=mydb"
    params:
      storage:
        data:
          mount: /var/lib/postgresql/data
          readOnly: false
profiles:
  compute:
    postgres:
      resources:
        cpu:
          units: 4.0
        memory:
          size: 8Gi
        storage:
          - size: 20Gi
          - name: data
            size: 20Gi
            attributes:
              persistent: true
              class: beta3
  placement:
    akash:
      attributes:
        host: akash
      signedBy:
        anyOf:
          - "akash1365yvmc4s7awdyj3n2sav7xfx76adc6dnmlx63"
      pricing:
        postgres:
          denom: uakt
          amount: 10000
deployment:
  postgres:
    akash:
      profile: postgres
      count: 1
```

Adjust the CPU, HDD, and other settings according to your needs.

After deployment, add the login to the `.env` file:

```bash
POSTGRES_URL=postgresql://eliza:yourpassword@provider.akash.ddns.net:yourportassigned/eliza_db
```

Use `pgadmin4` to access and backup the database via web browser.

## Run a Dedicated AKASH Engine

For temporary use, consider running a dedicated AKASH engine.


Llama 3.1 70B 1.65$ per hour ((2x A100) ----------
LLama 3.1 405B 8$ per hour   (8x A100)

Use this YAML to deploy:


## Cosmos Transaction Module

For now, it only supports tx bank transfers. With SKIP API, it will be able to IBC, trade, and stake tokens.

- [Discord](https://discord.gg/ai16z). Best for: sharing your applications and hanging out with the community.

## Contributors

<a href="https://github.com/elizaos/eliza/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=elizaos/eliza" />
</a>

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=elizaos/eliza&type=Date)](https://star-history.com/#elizaos/eliza&Date)
