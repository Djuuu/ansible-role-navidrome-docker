Ansible Role: Navidrome-docker
==============================

Install Navidrome Docker Compose project.

Navidrome is an open source web-based music collection server and streamer.

- https://www.navidrome.org/
- https://github.com/navidrome/navidrome/

Requirements
------------

Requires the following to be installed:
- docker
- docker compose

Role Variables
--------------

Common Docker projects variables:

```yaml
# Base directory for Docker projects
docker_projects_path: # /var/apps
```

Available role variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
# Docker project variables

navidrome_project_name: navidrome

# Docker project dynamic vars (uses `docker_project_name` prefix, adapt if overriden)

# Port targeted by Traefik router
navidrome_traefik_loadbalancer_server_port: 4533

# Main service additional docker-compose options (ex: cpu_shares, deploy, ...)
navidrome_compose_service_additional_options: |
  #ports:
  #  - "4533:4533"
```

```yaml
# Navidrome project variables

# deluan/navidrome container version
navidrome_version: latest

# Path to music collection
navidrome_music_volume: ./music

# Exposed service base URL
navidrome_baseurl: "https://music.example.net"


# Configure periodic scans using “cron” syntax. To disable it altogether, set it to "0"
navidrome_scanschedule: 1h

# Log level. Useful for troubleshooting. Possible values: error, warn, info, debug, trace
navidrome_loglevel: info

# How long Navidrome will wait before closing web ui idle sessions
navidrome_sessiontimeout: 24h
```

Dependencies
------------

This role depends on :
- [djuuu.docker_project](https://github.com/Djuuu/ansible-role-docker-project)

Some variables allow integration with:
- [djuuu.traefik_docker](https://github.com/Djuuu/ansible-role-traefik-docker)

Example Playbook
----------------

```yaml
- hosts: all
  gather_facts: true
  gather_subset:
    - "!all"
    - "!min"
    - user_id

  roles:
    - djuuu.navidrome_docker
```

License
-------

Beerware License
