# anki-build

I use this repository to build Docker images for Anki Sync Server as the
upstream does not offer an "official" image.

The image is created from [Anki's official Dockerfile](https://github.com/ankitects/anki/blob/main/docs/syncserver/Dockerfile).

## Using Hashed Password

```
$ docker run \
  -d \
  --restart always \
  -e "PASSWORDS_HASHED=1" \
  -e "SYNC_USER1=${username}:${password_hash}" \
  -p 8080:8080 \
  -v ${anki_sync_storage}:/home/anki/.syncserver \
  --name anki-sync-server \
  phbjdocker/anki-sync-server
```

Note to replace the following variables:

| Variable | Meaning |
| --- | --- |
| `${username}` | User name |
| `${password_hash}` | Hashed password, see [Hashed Passwords][anki-hashed-password] |
| `${anki_sync_storage}` | Persistent server data storage, see [Storage Location](https://docs.ankiweb.net/sync-server.html#storage-location). And make sure other users have write permission. |

## Password in Plain Text

A more convenient way using a plain-text password, by not setting the environment variable `PASSWORDS_HASHED`.

## Setting up Using Docker Compose

Follow these steps to set up your own servers:

```bash
git clone https://github.com/bvlgah/anki-build.git
cd anki-build
cp template.env .env # Remember to change username and password accordingly
docker compose -f anki-sync-server-plain.yml --env-file .env up -d
```

It is possible to set up a server using hashed passwords:

```bash
docker compose -f anki-sync-server.yml --env-file .env up -d
```

You can create a hash password by the technique mentioned in [Hashed Passwords][
anki-hashed-password].

[anki-hashed-password]: https://docs.ankiweb.net/sync-server.html#hashed-passwords
