# devgtv.docker

Bar widget for the Omarchy Shell (Quickshell) that shows running Docker containers and lets you adjust the RAM limit of each one.

## Installation

The plugin lives in `~/.config/omarchy/plugins/devgtv.docker/`. Enable it with:

```bash
omarchy plugin enable devgtv.docker right
```

The bar hot-reloads on save. To summon the popup from the keyboard you can bind the `devgtv.docker` IPC target to a Hyprland key.

## Usage

- Docker icon in the bar.
- Click to open the popup listing:
  - container name and image
  - status, CPU and used memory
  - RAM limit slider (128 MiB steps, from 128 MiB up to the host's total RAM)
- Keyboard (while the popup is focused):
  - `j`/`k` move between containers
  - `h`/`l` adjust the RAM limit (300 ms debounce)
  - `r` force refresh
  - `Esc` close

## Configuration

In `~/.config/omarchy/shell.json`, the widget entry accepts a custom `refreshMs`:

```json
{
  "id": "devgtv.docker",
  "refreshMs": 5000
}
```

Default is 3000 ms.

## Implementation details

- Data is collected by a Quickshell `Process` running `docker ps`, `docker inspect` and `docker stats`.
- The limit change uses `docker update --memory <MB>m --memory-swap -1 <container>`.
- The user must be in the `docker` group so `sudo` is not needed.

## Testing Model.js locally

You can test the parser with Node.js:

```bash
node -e '
const M = require("./Model.js");
console.log(M.snapshotScript);
console.log(M.parseSnapshot(require("child_process").execSync(M.snapshotScript, {encoding:"utf8"})));
'
```

## License

MIT
