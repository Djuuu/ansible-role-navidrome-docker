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

# Docker project dynamic vars (uses `docker_project_name` prefix, adapt if overridden)

# Port targeted by Traefik router
navidrome_traefik_loadbalancer_server_port: 4533

# Main service additional docker-compose options (ex: cpu_shares, deploy, ...)
navidrome_service_additional_options: |
  #ports:
  #  - "4533:4533"
```

Base configuration:

```yaml
# Navidrome project variables

# deluan/navidrome container version
navidrome_version: latest

# UID container is running as
navidrome_puid: "{{ ansible_user_uid }}"
# GID container is running as
navidrome_pgid: "{{ ansible_user_gid }}"


# Exposed service base URL
navidrome_baseurl: "https://music.example.net"

# Path to music collection on host
navidrome_music_volume: ./music

# Path to library backup directory on host
# (also set ND_BACKUP_PATH, ND_BACKUP_SCHEDULE and ND_BACKUP_COUNT in navidrome_env)
navidrome_backup_volume: # ex: ./backup


# Log level. Useful for troubleshooting. Possible values: error, warn, info, debug, trace
navidrome_loglevel: info

# Enable/disable the scanner. Set to false to disable automatic scanning of the music library.
navidrome_scanner_enabled: true
# Schedule for automatic scans. Use Cron syntax
navidrome_scanner_schedule: 0 # (disabled)

# How long Navidrome will wait before closing web ui idle sessions
navidrome_sessiontimeout: 24h

# Controls whether the server will run its Anonymous Data Collection feature to help improve the project.
navidrome_enableinsightscollector: true
```

Advanced configuration:  
(see https://www.navidrome.org/docs/usage/configuration-options/#environment-variables)

```yaml
navidrome_env:

  ## Change how album play count is computed. When set to "normalized", album play count will be divided by the number of album tracks
  # ND_ALBUMPLAYCOUNTMODE: "absolute"

  ## How many login requests can be processed from a single IP during the AuthWindowLength. Set to 0 to disable the limit rater
  # ND_AUTHREQUESTLIMIT: 5
  ## Window Length for the authentication rate limit
  # ND_AUTHWINDOWLENGTH: "20s"

  ## Enable/disable .m3u playlist auto-import
  # ND_AUTOIMPORTPLAYLISTS: true
  ## Set imported playlists as public by default
  # ND_DEFAULTPLAYLISTPUBLICVISIBILITY: false
  ## Configure the order to look for artist images.
  # ND_ARTISTARTPRIORITY: "artist.*, album/artist.*, external"

  ## Path to store backups. Set to "" to disable backups.
  # ND_BACKUP_PATH: "/data/backup"
  ## Schedule for automatic backups. Use Cron syntax
  # ND_BACKUP_SCHEDULE: "15 6 * * 6" # At 06:15 on Saturday
  ## Number of backups to keep
  # ND_BACKUP_COUNT: 5

  ## Configure the order to look for cover art images. Use special embedded value to get embedded images from the audio files
  # ND_COVERARTPRIORITY: "cover.*, folder.*, front.*, embedded, external"
  ## Set JPEG quality percentage for resized cover art images
  # ND_COVERJPEGQUALITY: 75

  ## Format to transcode to when client requests downsampling (specify maxBitrate without a format)
  # ND_DEFAULTDOWNSAMPLINGFORMAT: "opus"

  ## Sets the default language used by the UI when logging in from a new browser. This value must match one of the file names in the resources/i18n. Ex: for Chinese Simplified it has to be zh-Hans (case sensitive)
  # ND_DEFAULTLANGUAGE: "en"
  ## Sets the default theme used by the UI when logging in from a new browser. This value must match one of the options in the UI
  # ND_DEFAULTTHEME: "Dark"

  ## Enable image pre-caching of new added music
  # ND_ENABLEARTWORKPRECACHE: true
  ## Controls whether the player in the UI will animate the album cover (rotation)
  # ND_ENABLECOVERANIMATION: true
  ## Enable the option in the UI to download music/albums/artists/playlists from the server
  # ND_ENABLEDOWNLOADS: true
  ## Set this to false to completely disable ALL external integrations, including the anonymous data collection and the nice login background images
  # ND_ENABLEEXTERNALSERVICES: true
  ## Enable toggling “Heart”/“Loved” for songs/albums/artists in the UI (maps to “Star”/“Starred” in Subsonic Clients)
  # ND_ENABLEFAVOURITES: true
  ## Use Gravatar images as the user profile image. Needs the user’s email to be filled
  # ND_ENABLEGRAVATAR: false
  ## Whether or not sensitive information (like tokens and passwords) should be redacted (hidden) in the logs
  # ND_ENABLELOGREDACTING: true
  ## If set to false, it will return the album CoverArt when a song CoverArt is requested
  # ND_ENABLEMEDIAFILECOVERART: true
  ## Enable ReplayGain options in the UI
  # ND_ENABLEREPLAYGAIN: true
  ## Enable the Sharing feature
  # ND_ENABLESHARING: false
  ## Enable 5-star ratings in the UI
  # ND_ENABLESTARRATING: true
  ## Enables transcoding configuration in the UI
  # ND_ENABLETRANSCODINGCONFIG: false
  ## Enable regular users to edit their details and change their password
  # ND_ENABLEUSEREDITING: true

  ## Path to ffmpeg executable. Use it when Navidrome cannot find it, or you want to use a specific version
  # ND_FFMPEGPATH: # Empty (search in the PATH)

  ## Send basic info to your own Google Analytics account. Must be in the format UA-XXXXXXXX
  # ND_GATRACKINGID: # Empty (disabled)

  ## Allows the X-Frame-Options header value to be set with a custom value. Ex: "SAMEORIGIN"
  # ND_HTTPSECURITYHEADERS_CUSTOMFRAMEOPTIONSVALUE: "DENY"

  ## List of ignored articles when sorting/indexing artists
  # ND_IGNOREDARTICLES: "The El La Los Las Le Les Os As O A"

  ## Size of image (art work) cache. Set to "0" to disable cache
  # ND_IMAGECACHESIZE: "100MB"

  ## Enable Jukebox mode (play audio on server’s hardware)
  # ND_JUKEBOX_ENABLED: false
  ## By default, Jukebox mode is only available to Admins. Set this option to false to allow any valid user to control it
  # ND_JUKEBOX_ADMINONLY: true
  ## Device to use for Jukebox mode, if there are multiple Jukebox.Devices entries.
  # ND_JUKEBOX_DEFAULT: # Empty (auto detect)

  ## Set this to false to completely disable Last.fm integration
  # ND_LASTFM_ENABLED: true
  ## Last.fm API Key
  # ND_LASTFM_APIKEY: # Empty
  ## Last.fm API Secret
  # ND_LASTFM_SECRET: # Empty
  ## Two letter-code for language to be used to retrieve biographies from Last.fm
  # ND_LASTFM_LANGUAGE: "en"

  ## Set this to override the default ListenBrainz base URL (useful with self-hosted solutions like Maloja*
  # ND_LISTENBRAINZ_BASEURL: "https://api.listenbrainz.org/1/"
  ## Set this to false to completely disable ListenBrainz integration
  # ND_LISTENBRAINZ_ENABLED: true

  ## Set the maximum number of playlists shown in the UI’s sidebar. Note that a very large number can cause UI performance issues.
  # ND_MAXSIDEBARPLAYLISTS: 100

  ## Path to mpv executable. Used for Jukebox mode
  # ND_MPVPATH: # Empty (search in PATH)

  ## Cmd template used to construct the call of the mpv executable. Used for Jukebox mode
  # ND_MPVCMDTEMPLATE: "mpv --audio-device=%d --no-audio-display --pause %f --input-ipc-server=%s"

  ## Passphrase used to encrypt passwords in the DB.
  # ND_PASSWORDENCRYPTIONKEY: # -

  ## Set the tag(s) to use as the Album ID.
  # ND_PID_ALBUM: "musicbrainz_albumid|albumartistid,album,albumversion,releasedate"
  ## Set the tag(s) to use as the Track ID.
  # ND_PID_TRACK: "musicbrainz_trackid|albumid,discnumber,tracknumber,title"

  ## Limit where to search for and import playlists from. Can be a list of folders/globs (separated by : (or ; on Windows).
  ## Paths MUST be relative to MusicFolder
  # ND_PLAYLISTSPATH: # Empty (meaning any playlist files in your library will be imported)

  ## Use Sort_* tags to sort columns in the UI.
  # ND_PREFERSORTTAGS: false

  ## Enable extra endpoint with Prometheus metrics.
  # ND_PROMETHEUS_ENABLED: false
  ## Custom path for Prometheus metrics. Useful for blocking unauthorized metrics requests.
  # ND_PROMETHEUS_METRICSPATH: "/metrics"

  ## Uses music files’ modification time when sorting by “Recently Added”. Otherwise use import time
  # ND_RECENTLYADDEDBYMODTIME: false

  ## HTTP header containing the user name from an authenticating proxy.
  # ND_REVERSEPROXYUSERHEADER: "Remote-User"
  ## Comma separated list of IP CIDRs (or when listening on a UNIX socket the special value @) which are allowed to use reverse proxy authentication.
  ## Empty means “deny all”. Note: This option is unnecessary for most reverse proxy setups, only for authenticating reverse proxies.
  # ND_REVERSEPROXYWHITELIST: # Empty

  ## Enable/disable the scanner. Set to false to disable automatic scanning of the music library.
  # #ND_SCANNER_ENABLED: true
  ## Schedule for automatic scans. Use Cron syntax
  # #ND_SCANNER_SCHEDULE: 0 # (disabled)
  ## Time to wait after a file change is detected before starting a scan. Useful to avoid scanning incomplete files.
  ## Set it to 0 to disable the watcher
  # ND_SCANNER_WATCHERWAIT: "5s"
  ## Enable/disable scanning the music library on startup. Set to false to disable
  # ND_SCANNER_SCANONSTARTUP: true

  ## Match query strings anywhere in searchable fields, not only in word boundaries. Useful for languages where words are not space separated
  # ND_SEARCHFULLSTRING: false

  ## How long Navidrome will wait before closing web ui idle sessions
  # #ND_SESSIONTIMEOUT: "24h"

  ## Base URL for shared links. Useful when your server address is not a public (ex: when using Tailscale). See discussion here
  # ND_SHAREURL: # Empty (use server address)

  ## How often to refresh Smart Playlists. Check the smart playlists docs
  # ND_SMARTPLAYLISTREFRESHDELAY: "5s"

  ## Spotify Client ID. Required if you want Artist images
  # ND_SPOTIFY_ID: # Empty
  ## Spotify Client Secret. Required if you want Artist images
  # ND_SPOTIFY_SECRET: # Empty

  ## Append the subtitle tag to the song title in all Subsonic API responses
  # ND_SUBSONIC_APPENDSUBTITLE: true
  ## When Subsonic clients request artist’s albums, include albums where the artist participates (ex: Various Artists compilations)
  # ND_SUBSONIC_ARTISTPARTICIPATIONS: false
  ## Set to true to report the real path of the music files in the API. Can be customized individually for each client/player.
  ## This can be a security risk, so it is disabled by default
  # ND_SUBSONIC_DEFAULTREPORTREALPATH: false
  ## List of clients that does not work with the new OpenSubsonic API improvements.
  # ND_SUBSONIC_LEGACYCLIENTS: "DSub"

  ## Path for the TLS certificate file, which should include the signature chain if any
  # ND_TLSCERT: # Empty (disable TLS)
  ## Path for the TLS key file
  # ND_TLSKEY: # Empty (disable TLS)

  ## Size of transcoding cache. Set to "0" to disable cache
  # ND_TRANSCODINGCACHESIZE: "100MB"

  ## Change background image used in the Login page
  # ND_UILOGINBACKGROUNDURL: # random music image from this Unsplash.com collection
  ## Add a welcome message to the login screen
  # ND_UIWELCOMEMESSAGE: # Empty

  ## Set file permissions for Unix Socket File.*
  # ND_UNIXSOCKETPERM: "0660"
```

Dependencies
------------

This role depends on :
- [djuuu.docker_project](https://github.com/Djuuu/ansible-role-docker-project)

Some variables allow integration with:
- [djuuu.traefik_docker](https://github.com/Djuuu/ansible-role-traefik-docker)

Example Playbook
----------------

Install Navidrome:

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

Scan library:

```yaml
- hosts: all
  gather_facts: false

  tasks:
    - name: Scan library
      ansible.builtin.include_role: { name: djuuu.navidrome_docker, tasks_from: scan }
```

```bash
ansible-playbook navidrome-scan-library.yml -e full=true
```

License
-------

Beerware License
