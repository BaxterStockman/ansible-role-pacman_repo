# Ansible Role - Pacman Repo

Set up a Pacman repository.  This role installs the
[darkhttpd](http://www.freecode.com/projects/darkhttpd) webserver for serving
packages, and also installs [aurutils](https://github.com/AladW/aurutils) for
building packages and synchronizing with the Arch User Repository.

## Requirements

This role requires the `usermod` utility, which is used for locking the account
used for package building via `usermod --lock {{ pacman_repo_user }}`.

## Role Variables

## Dependencies

- `pacman_repo_name`: The name of your repository.  This variable is mandatory
  and has no default value.
- `pacman_repo_root`: Where to store packages.  Defaults to
  `/var/cache/{{ pacman_repo_name }}`.
- `pacman_repo_user`: The user account to be used for building packages.
  Defaults to `build`.  This account will be created if it does not already
  exist.
- `pacman_repo_group`: The group to be used for building packages.
- `pacman_repo_port`:  The port the darkhttpd webserver will listen on.
- `pacman_repo_environment_file`: A shell profile that will be used as a
  systemd `EnvironmentFile` in `pacman_repo_service`.  Defaults to
  `/etc/conf.d/{{ pacman_repo_name }}`.
- `pacman_repo_config_file`: The destination path for a custom pacman
  configuration file for the created repository.  Defaults to
  `/etc/pacman.d/{{ pacman_repo_name }}.conf`.
- `pacman_repo_service`:  The name of the systemd service for running the
  repository webserver.  Defaults to `{{ pacman_repo_name }}.service`.
- `pacman_repo_aursync_service`:  The name of the systemd service for
  synchronizing the repository against the AUR.  Defaults to
  `aursync@.service`.
- `pacman_repo_aursync_timer`: The name of the systemd timer for periodically
  synchronizing the repository against the AUR.  Defaults to `aursync@.timer`.
- `pacman_repo_calendar_event_expr`:  The calendar event expression to be used
  as the value of `OnCalendar` in the `pacman_repo_aursync_timer` systemd
  timer.

## Example Playbook

```yaml
- hosts: repo
  roles:
    - name: BaxterStockman.pacman_repo
      pacman_repo_port: 8080
      pacman_repo_calendar_event_expr: weekly
```

License
-------

MIT

Author Information
------------------

Matt Schreiber <schreibah@gmail.com>
