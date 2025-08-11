![Gemini cli image](https://github.com/claytronOnLinux/gemini-cli/blob/main/assets/gemini.png)

# Gemini CLI Docker Environment

This project provides a simple, clean Dockerized environment for running the Google Gemini CLI (`@google/gemini-cli`).

## Configuration

### API Key and Work Directory

Create a `.env` file in this directory and add your Gemini API key:
Also add a volume to use as your /work directory:
```bash
GEMINI_API_KEY=your_api_key_here
WORK_DIR=/home/my_username/work
```

### Compose file

Create or clone a compose file with the following:

```bash
services:
  gemini-cli:
    build: .
    image: claytrononlinux/gemini-cli
    container_name: gemini-cli
    hostname: gemini-cli
    # Via a .env set a volume mount for a local directory into the container's /work directory.
    # When you enter the contianer, navigate to that directory to work out of.
    volumes:
      - ${WORK_DIR}:/work
    user: "1000:1000"
    env_file:
      - ./.env
    # tty Keeps the container running in the background
    tty: true
```

### File Permissions

By default, the container runs as a non-root user named `user`. This is a good security practice, but it can cause files created in the mounted `./data` volume to have incorrect ownership on your host machine.

To fix this, you can tell the container to run as your own user and group. Open the `compose.yaml` file and uncomment the `user` directive, replacing `1000:1000` with your own user and group ID.

```yaml
# In compose.yaml
# ...
    # The user and group to run the container as.
    # This overrides the default 'user' and is useful for ensuring that
    # files created in the mounted volume have the correct ownership.
    # Example: user: "1000:1000"
    user: "1000:1000"
# ...
```

You can find your user and group ID by running the `id` command in your terminal.

## Usage

1.  **Start and Connect**: 
    ```bash
    docker compose up -d
    ```
    ```bash
    docker exec -it gemini-cli /bin/bash
    ```
    ```bash
    cd /work
    ```


