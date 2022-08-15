# Docker Compose

`docker-compose up --build` - re-builds the previously built images which are cached and reused if `--build` switch is skipped

Docker compose creates the "bridge" network automatically so the containers can communicate with one another across it.
- The service names in the `docker-compose.yml` file act as DNS

What is the "build context" in a compose file supposed to do? (See sample 3)
- It is meant to specify where the Dockerfile of the image is supposed to build from
- the "." indicates the dockerfile exists in the same directory as the compose file

The `ports:` key in a compose file does the same thing as `EXPOSE` stanza in a Dockerfile?
- False 
- the "ports:" key actually publishes the particular service on whatever port you specify, and is the docker run equivalent to the -p flag.
- whereas, the `EXPOSE` stanza does not expose/publish any ports at all, it must be done at runtime in the `docker run...` command. 
    - The `EXPOSE` stanza is only meant as a documentation/reminder

Sample 1:

```yaml
services:
  server:
    image: drupal:9.3.13
    ports:
      - "8080:80"
    environment:
      POSTGRED_DB: database
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: abc123
    volumes:
      - drupal-modules:/var/www/html/modules
      - drupal-profiles:/var/www/html/profiles
      - drupal-sites:/var/www/html/sites
      - drupal-themes:/var/www/html/themes
  database:
    image: postgres:14.3
    environment:
      POSTGRES_PASSWORD: abc123

volumes:
  drupal-modules:
  drupal-profiles:
  drupal-sites:
  drupal-themes:

```

Sample: 2  Docker file with Docker Compose 

```dockerfile
FROM drupal:9

RUN apt-get update && apt-get install -y git

RUN rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html/themes

RUN git clone --branch 8.x-4.x --single-branch --depth 1 https://git.drupalcode.org/project/bootstrap.git \
    && chown -R www-data:www-data bootstrap

WORKDIR /var/www/html
```

```yaml
services:
  drupal:
    image: custom-drupal
    build: .
    ports:
      - "8080:80"
    environment:
      POSTGRED_DB: database
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: abc123
    volumes:
      - drupal-modules:/var/www/html/modules
      - drupal-profiles:/var/www/html/profiles
      - drupal-sites:/var/www/html/sites
      - drupal-themes:/var/www/html/themes
  database:
    image: postgres:14.3
    environment:
      POSTGRES_PASSWORD: abc123
    volumes:
      - drupal-data:/var/lib/postgresql/data

volumes:
  drupal-modules:
  drupal-profiles:
  drupal-sites:
  drupal-themes:
  drupal-data:

```

Sample 3: 

```yaml
services:
  proxy:
    build:
      context: .
      dockerfile: nginx.Dockerfile
    ports:
      - '80:80'
  web:
    image: httpd
    volumes:
      - ./html:/usr/local/apache2/htdocs/

```