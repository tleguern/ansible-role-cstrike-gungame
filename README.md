# Ansible Role: Counter-Strike: Source GunGame mode

An Ansible role that installs, upgrades and configures the gungame mode for Counter-Strike: Source.

## Requirements

An ansible role dedicated to the installation of SteamCMD such as [ansible-steamcmd](https://github.com/tleguern/ansible-steamcmd).

An ansible role dedicated to the Installation of Metamod:Source such as [ansible-role-metamod-source](https://github.com/tleguern/ansible-role-metamod-source).

An ansible role dedicated to the installation of a Source mod such as [ansible-role-cstrike-source](https://github.com/tleguern/ansible-role-cstrike-source) or any role providing the `Restart cstrike-source` handler.

## Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `steamcmd_user` | User name for steamcmd | `steam` |
| `gungame_url` | Download mirror | `https://github.com/altexdim/sourcemod-plugin-gungame/archive/refs/heads/master.tar.gz` |
| `gungame_version` | Desired version | `head` |
| `gungame_target` | Archive name | `sourcemod-plugin-gungame-master.tar.gz` |
| `gungame_install_path` | Installation directory | `/home/{{ steamcmd_user }}/cstrike-source/cstrike` |
| `gungame_config_txt` | Global GunGame configuration | `""` |
| `gungame_maps_cfg` | Per map GunGame configuration | `[]` |

### `gungame_config_txt`

By default the stock configuration file is kept but it is possible to specify a different one like this:

```yaml
gungame_config_txt: |
  "GunGame.Config"
  {
      "Config"
      {
          "Enabled" "0"
      }
  }
```

### `gungame_maps_cfg`

Map specific configuration are possible using this variable.
It is a list of dictionary using two keys: `map` and `cfg`.
The first one is the partial or full name of a map and the second the specific configuration.

For example the following configuration enable the GunGame mode on every map prefixed with the string `gg`:

```yaml
gungame_maps_cfg:
  - map: gg
    cfg: |
      "GunGame.Config"
      {
          "Config"
          {
              "Enabled" "1"
          }
      }

```

## Dependencies

None.

## Example Playbook

```yaml
- hosts: game
  pre_tasks:
    - package:
        name: acl
        state: present
  roles:
    - role: tleguern.steamcmd
    - role: tleguern.cstrike-source
    - role: tleguern.metamod-source
    - role: tleguern.sourcemod
    - role: tleguern.cstrike-gungame
```

## License

ISC

## Contributing

Either send [send GitHub pull requests](https://github.com/tleguern/ansible-role-cssdm) or [send patches on SourceHut](https://lists.sr.ht/~tleguern/misc).

## Author Information

Tristan Le Guern <tleguern@bouledef.eu>
