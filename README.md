# gentoo-pkg-builder

Use a Docker environment as package builder. Useful to delegate building to a different host,
or keep your workstation in a consistent state while building packages, after which you need to only
install binary packages.

This approach uses the official [`gentoo/stage3`](https://hub.docker.com/r/gentoo/stage3) Docker images,
confuration based on `/etc/portage` and the commandline in `docker-compose.yml`.

Upon running a `cache` directory will be created and mounted on the host system,
containing the downloaded `distfiles` and built `binpks`.
Using the default commandline, each run will `--emptytree` which will rebuild the base system packages from the upstream Docker image to match the local `CFLAGS` options. With the `--usepkg` option previously built packages will be reused if they match the current `USE` flags (through the `--usepkg` flag).

**Note:** it is advised to delete the `cache/binpkg` directory when `CFLAGS` are changed to allow for a full rebuild. Some users might also want to do this after GCC upgrades.

## How to use

You need Docker with the [Compose plugin](https://docs.docker.com/compose/install/compose-plugin/).

1. Fork and clone this repo;
2. Modify `portage/` files to reflect your system needs. (`CFLAGS`, `USE` flags, `package.accept_keywords`, etc.);
3. (optional) Add packages to a file in `portage/sets`;
4. Run `docker compose run gentoo [package...] || [@set...]`;

For example, this repository already defines a `core` set with some primary admin tools, kernel and bootloader:

```shell
docker compose run gentoo @core
```
