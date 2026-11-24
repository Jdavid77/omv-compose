# omv-compose

Docker Compose stacks for the OpenMediaVault NAS, managed by the openmediavault-compose plugin.
The cluster in [home-cluster](https://github.com/Jdavid77/home-cluster) depends on some of them.

| Stack | Role |
|---|---|
| `postgres18/` | Shared PostgreSQL instance. Cluster apps keep their databases here |
| `vectorchord/` | pgvector-compatible Postgres for vector workloads |
| `pihole/` | DNS server for the local network |
| `exit-node/` | Tailscale exit node through NordVPN. Builds the `tailnord` image from its `Dockerfile` |

## Layout

The repo is checked out as the plugin's Compose Files shared folder. Each stack is
`<name>/<name>.yml`, and its secrets go in `<name>/<name>.env` next to it, which you can edit in
the plugin UI. The stacks load that file through the `.env` symlink the plugin creates.

The `.env` files and the plugin's generated `compose.yml` and `compose.override.yml` are
gitignored and stay on the NAS. `<name>.env.example` lists the keys each stack needs.

Data lives in the `appdata` shared folder. The stacks reference it as `${APPDATA}`, which is set in
`global.env`, the plugin's global environment file. That file is committed, so keep secrets out of
it, and edit it here rather than in the plugin UI. Compose refuses to start a stack if `APPDATA` is
unset.

Don't use the plugin's `${{ sf:"..." }}` or `CHANGE_TO_COMPOSE_DATA_PATH` placeholders. The plugin
only replaces them in files saved from its editor, not in files pulled from git.

## Updating

Renovate opens a PR for each image update. After merging:

1. On the NAS, run `git pull` in the Compose Files shared folder.
2. In the plugin, select the stack, then **Sync changes from file** and **Up**.
