# docker-aws-cli

This setup runs the AWS CLI Docker image as a long-lived container using
Docker Compose, with AWS credentials pre-loaded from your `.env` file.
You can drop into an interactive shell or run one-off AWS commands via `docker exec`.

## How It Works

- `image`: uses the official AWS CLI Docker image.
- `entrypoint`: overrides the default entrypoint with `tail -f /dev/null` to keep the container alive.
- `working_dir`: sets `/workspace` as the default directory inside the container.
- `env_file`: loads AWS credentials and configuration from a `.env` file.

## Environment Variables

To authenticate with AWS CLI:

1. Create a `.env` file from the example:
   ```bash
   cp .env.example .env
   ```
2. Populate the required environment variables:
   ```plaintext
   AWS_ACCESS_KEY_ID=
   AWS_SECRET_ACCESS_KEY=
   AWS_SESSION_TOKEN=
   AWS_DEFAULT_REGION=
   ```

## Usage

Start the container in the background:

```bash
docker compose up -d
```

### Interactive Shell

Drop into a shell to run commands sequentially:

```bash
docker exec -it <container-name> sh
```

### One-Off Commands

Run AWS commands directly:

- `docker exec <container-name> aws s3 ls`
- `docker exec <container-name> aws sts get-caller-identity`
- `docker exec <container-name> aws ec2 describe-instances`

### Mounting a Volume

To give the container access to local files, add a `volumes` entry to `docker-compose.yml`:

```yaml
services:
  aws-cli:
    image: amazon/aws-cli:latest
    working_dir: /workspace
    entrypoint: ["tail", "-f", "/dev/null"]
    volumes:
      - ./:/workspace
    env_file:
      - .env
```

This mounts `./` from the host into `/workspace` inside the container.

---

> **Note**: Stop and remove the container with `docker compose down`.

## Notes

- Ideal for local development and learning.
- No need to install AWS CLI locally.
- The container stays running until you stop it.
