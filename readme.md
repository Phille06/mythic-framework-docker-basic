# 🎮 Mythic Framework Docker

> One-command FiveM server with txAdmin + Mythic Framework — fully automated setup.

---

## Requirements

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running
- A free Cfx.re account at [keymaster.fivem.net](https://keymaster.fivem.net)

---

## Quick Start

### 1 — Download the files

Download these two files and place them in the same folder:

- [`docker-compose.yml`](./docker-compose.yml)
- [`.env.example`](./.env.example)

---

### 2 — Get your license key

1. Go to [keymaster.fivem.net](https://keymaster.fivem.net)
2. Click **New Server** and enter your server's public IP
3. Copy the full key — it starts with `cfxk_`

> **Don't know your public IP?** Run this in a terminal:
> ```bash
> curl https://api.ipify.org
> ```

---

### 3 — Configure your server

Rename `.env.example` to `.env`, open it in any text editor, and fill in these values:

| Variable | Description |
|---|---|
| `MYSQL_ROOT_PASSWORD` | Any secure password for the database root account |
| `MYSQL_PASSWORD` | Any secure password for the FiveM database user |
| `MONGO_INITDB_ROOT_PASSWORD` | Any secure password for MongoDB |
| `SERVER_LICENSE_KEY` | Your `cfxk_...` key from Step 2 |
| `SERVER_NAME` | Your server's display name |

Leave everything else as default unless you know what you're changing.

---

### 4 — Start the server

Open a terminal in the folder containing your files and run:

```bash
docker compose up -d
```

> ⏳ **First start takes 5–15 minutes.** It will automatically download FXServer and all Mythic Framework resources. Grab a coffee.

---

### 5 — Open txAdmin

Once the server is running, open your browser and go to:

```
http://YOUR-SERVER-IP:40120
```

---

## Useful Commands

```bash
# View live server logs
docker logs fivem_server -f

# Stop everything
docker compose down

# Restart just the FiveM server
docker compose restart fivem

# Update to the latest image
docker compose pull && docker compose up -d
```

---

## File Structure

After first start, your folder will look like this:

```
your-folder/
├── docker-compose.yml
├── .env
└── data/
    ├── mariadb/        ← MariaDB database files
    ├── mongodb/        ← MongoDB database files
    ├── serverfiles/    ← FXServer + all resources + server.cfg
    └── txData/         ← txAdmin config, bans, logs, whitelist
```

All your server data lives in `./data/` — back this folder up to save your server.

---

## Ports

| Port | Purpose |
|---|---|
| `30120` TCP + UDP | FiveM game traffic |
| `40120` TCP | txAdmin web panel |

Make sure these are open in your firewall / hosting provider.

---

## Troubleshooting

**txAdmin shows a PIN setup wizard instead of login**
> Delete `./data/txData/` and restart. The pre-configuration will be reapplied.

**Server fails to authenticate license key**
> Make sure the IP on [keymaster.fivem.net](https://keymaster.fivem.net) matches your server's public IP. Run `curl https://api.ipify.org` inside the container to verify:
> ```bash
> docker exec fivem_server curl -s https://api.ipify.org
> ```

**Force a full fresh re-deploy**
> ```bash
> docker compose down
> # Delete the lock file:
> rm ./data/serverfiles/.mythic_deploy_complete
> docker compose up -d
> ```

---

## Security

- **Never commit your `.env` file** — it contains passwords and your license key. It is already listed in `.gitignore`.
- Database ports (3306, 27017) are **not exposed** to the internet by default. Uncomment the `ports` sections in `docker-compose.yml` only if you need external access.

---

## Credits

- FiveM Docker base image by [ich777](https://github.com/ich777/docker-fivem-server)
- Forked and extended by [Phille06](https://github.com/Phille06/mythic-framework-docker)
- [Mythic Framework](https://github.com/mythic-framework) by The Community