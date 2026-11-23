# omv-compose

Docker Compose stacks for the OpenMediaVault NAS, managed by the openmediavault-compose plugin.
The cluster in [home-cluster](https://github.com/Jdavid77/home-cluster) depends on some of them.

| Stack | Role |
|---|---|
| `postgres18/` | Shared PostgreSQL instance. Cluster apps keep their databases here |
| `vectorchord/` | pgvector-compatible Postgres for vector workloads |
| `pihole/` | DNS server for the local network |
| `exit-node/` | Tailscale exit node through NordVPN. Builds the `tailnord` image from its `Dockerfile` |

`scripts/renew-cert.sh` copies the `*.jnobrega.com` certificate from Akeyless onto the NAS.

## Layout

The repo is the plugin's Compose Files shared folder. Each stack is `<name>/<name>.yml`, with
its secrets in `<name>/<name>.env` next to it (editable in the plugin UI). The stacks load them
through the `.env` symlink the plugin creates. The `.env` files, and the plugin's `compose.yml`
and `compose.override.yml`, are gitignored and stay on the NAS; `<name>.env.example` lists the
keys. Data lives in the `appdata` shared folder, referenced as `${{ sf:"appdata" }}`, which the
plugin replaces with the absolute path.

## Updating

Renovate opens a PR for each image update. After merging:

1. On the NAS, run `git pull` in the Compose Files shared folder.
2. In the plugin, select the stack, then **Sync changes from file** and **Up**.
