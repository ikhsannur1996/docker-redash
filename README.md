# Redash Docker Setup with Auto-Restart

This guide provides step-by-step instructions for setting up a Redash instance using Docker with the added auto-restart option. Redash is an open-source data visualization and dashboarding platform that allows you to connect to various data sources and create interactive visualizations and dashboards.

## Prerequisites

Before you proceed, ensure you have the following installed on your system:

1. Docker
2. Docker Compose

## Steps to Set Up Redash with Auto-Restart

1. Clone the Redash Repository

```bash
git clone https://github.com/getredash/redash.git
cd redash
```

2. Create a Configuration File

Copy the sample `.env` file and edit it to configure Redash settings:

```bash
cp .env.sample .env
```

Edit the `.env` file as needed. You can set the `REDASH_SECRET_KEY`, `REDASH_COOKIE_SECRET`, `REDASH_DATABASE_URL`, and other configuration options.

3. Update the Docker Compose File

Open the `docker-compose.yml` file in a text editor, and add the `restart` option to each service to enable auto-restart:

```yaml
# Add the 'restart: always' line to each service
version: '3'
services:
  server:
    image: redash/redash:latest
    depends_on:
      - postgres
      - redis
    env_file:
      - ./env
    restart: always  # Enable auto-restart
    ports:
      - "5000:5000"

  scheduler:
    image: redash/redash:latest
    command: scheduler
    depends_on:
      - postgres
      - redis
    env_file:
      - ./env
    restart: always  # Enable auto-restart

  worker:
    image: redash/redash:latest
    command: worker
    depends_on:
      - postgres
      - redis
    env_file:
      - ./env
    restart: always  # Enable auto-restart

  redis:
    image: redis:latest
    restart: always  # Enable auto-restart

  postgres:
    image: postgres:9.6
    env_file:
      - ./postgres/env
    volumes:
      - ./postgres/data:/var/lib/postgresql/data
    restart: always  # Enable auto-restart
```

4. Build and Start the Docker Containers

Run the following command to download the required Docker images and start the Redash containers in the background:

```bash
docker-compose up -d
```

The `-d` flag runs the containers in detached mode.

5. Initialize the Database

Run the following command to initialize the Redash database:

```bash
docker-compose run --rm server create_db
```

6. Create an Administrative User (Optional)

If you want to create an administrative user, run the following command and follow the prompts:

```bash
docker-compose run --rm server create_user
```

7. Access Redash

Open your web browser and navigate to `http://localhost:5000` (or your server's IP address if you're deploying remotely) to access Redash.

8. Additional Configuration (Optional)

Depending on your requirements, you may need to perform additional configuration, such as setting up an email server for notifications or customizing the authentication method. Refer to the Redash documentation for more details.

9. Stopping Redash

To stop Redash and the associated Docker containers, run:

```bash
docker-compose down
```

## License

This project is licensed under the [MIT License](LICENSE).

## Acknowledgments

Redash is an amazing open-source project. Please consider supporting its development by contributing to the project or donating to the Redash team.

## Links

- Redash Website: https://redash.io/
- Redash GitHub Repository: https://github.com/getredash/redash

## Contact

If you encounter any issues or have questions regarding Redash Docker setup, feel free to reach out to us:

- Email: support@example.com
- Issue Tracker: https://github.com/getredash/redash/issues

---

This README document guides you through the process of setting up Redash using Docker with the auto-restart option enabled. Please follow the steps carefully, and don't hesitate to contact us if you need any assistance. Happy data visualization!
