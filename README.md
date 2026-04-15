# docker-aws-cli

This setup allows you to run an interactive shell inside a Docker container using
Docker Compose, with AWS credentials pre-loaded from your `.env` file.

## How It Works

- `image`: uses the official AWS CLI Docker image.
- `stdin_open` + `tty`: enables interactive terminal usage.
- `entrypoint`: overrides the default container entrypoint to launch a `sh` shell.
- `env_file`: loads AWS credentials and configuration from a `.env` file.

## Environment Variables

To authenticate with AWS CLI inside the shell:

1. Create a `.env` file from the example:
   ```bash
   mv .env.example .env
   ```
2. Populate the required environment variables:
   ```plaintext
   AWS_ACCESS_KEY_ID=
   AWS_SECRET_ACCESS_KEY=
   AWS_SESSION_TOKEN=
   AWS_DEFAULT_REGION=
   ```

## Usage

Start an interactive shell session:

```bash
docker compose run --rm aws-cli
```

### Examples

Once inside the shell, you can run AWS CLI commands directly:

- List S3 buckets:
  ```sh
  aws s3 ls
  ```
- Check current identity:
  ```sh
  aws sts get-caller-identity
  ```
- List EC2 instances:
  ```sh
  aws ec2 describe-instances
  ```

## Notes

- Ideal for local development and learning.
- No need to install AWS CLI locally.
- Each session runs in a temporary container (`--rm`).
