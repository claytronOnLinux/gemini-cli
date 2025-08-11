# Gemini CLI Docker Environment

This project provides a simple, clean Dockerized environment for running the Google Gemini CLI (`@google/gemini-cli`).

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

## Configuration

### API Key

Create a `.env` file in this directory and add your Gemini API key:
Also add a volume to use as your /work directory:
```bash
GEMINI_API_KEY=your_api_key_here
WORK_DIR=/home/my_username/work
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
