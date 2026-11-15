# eBlocker in Docker (fork flexi767/eblocker, branch `docker`)

This fork's `docker` branch is the source for a containerised eBlocker. The
Dockerfile, compose file, runit services, operator tooling and full docs live
in **[flexi767/eblocker-docker](https://github.com/flexi767/eblocker-docker)**
(private): see its `README.md` (build, run, rollback, upstream sync) and
`AGENTS.md` (operator guide).

## Changes on this branch (vs upstream `develop`)

* `eblocker-icapserver.sh`: honours `EBLOCKER_JAVA_MEMORY_SETTINGS`
  (a container's `free -m` shows the host's RAM, not its limit).
* `certificate-validator.sh`: honours `CERTVALIDATOR_JAVA_MEMORY_SETTINGS`.
* `squid.static`: `cache_mem 8 MB`, `memory_pools off` (caching is disabled anyway).
* this file.

Keep this branch small; container-only behaviour belongs in eblocker-docker.

## Operator rules (summary)

* Device settings (enabled devices, auto-enable of new devices) belong to the
  owner and are set in the web UI.
* Never change the host's networking (NIC, WireGuard, firewall persistence,
  DNS).
* eBlocker ARP-spoofs enabled devices. `docker compose stop eblocker` heals
  them; after a crash run `./heal <device-ip>` from the eblocker-docker
  directory.
* Access: `ssh eblocker` (user `v`, key only) from the LAN, or
  `sudo docker exec -it -u v eblocker bash -l` on the Docker host.

## Sync with upstream

```
git fetch upstream && git checkout docker && git merge upstream/develop && git push origin docker
```

then rebuild with `./build.sh` in eblocker-docker.
