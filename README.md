# OsQuery Playbook
**Not fully ready, deploying osquery with basic configs**

# Variables
```
# https://osquery.io/downloads/official
_apt_repo_key_id -> osquery apt repository gpg key (DEFAULT: 1484120AC4E9F8A1A577AEEE97A80C63C9D8B80B)(type: string)
_apt_repository -> osquery repository (DEFAULT: 'deb [arch=amd64] https://pkg.osquery.io/deb deb main')(type: string)

# https://osquery.readthedocs.io/en/stable/installation/cli-flags
_osquery_options -> osquery options (jsonpath: .options)
# DEFAULT
_osquery_options:
    host_identifiere: '"hostname"'


# https://osquery.readthedocs.io/en/latest/deployment/configuration/#query-packs
_osquery_packs -> osquery packs You can define (jsonpath: .packs)
# DEFAULT: empty, EXAMPLE:
_osquery_packs:
- pack_name: crontab
  content: |-
    {
      "queries": {
        "system_crons": {
            "query": "SELECT path, command FROM crontab;",
            "interval": 10
            }
        }
    }
- pack_name: users
  content: |-
    {
      "queries": {
        "users_count": {
          "query": "SELECT COUNT(1) FROM users;",
          "interval": 30
          },
        "users": {
          "query": "SELECT username, uid, gid, shell FROM users;",
          "interval": 60
          }
        }
    }

# https://osquery.readthedocs.io/en/latest/deployment/configuration/
_osquery_schedules -> schedules for queries (jsonpath: .schedule)
# DEFAULT: empty, EXAMPLE:
_osquery_schedules:
- schedule_name: files
  query: >-
    SELECT 
    u.username, g.groupname,
    f.path, f.inode, f.size,
    h.md5
    FROM file AS f
    INNER JOIN hash AS h USING(path)
    INNER JOIN users AS u USING(uid)
    INNER JOIN groups AS g ON u.gid=g.gid
    WHERE path='/etc/shadow';
  interval: 60
```
