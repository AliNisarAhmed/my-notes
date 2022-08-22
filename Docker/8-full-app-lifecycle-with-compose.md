# Full App Lifecycle with Compose & Swarm

Single set of compose files for: 
- prod
- staging
- test
- local dev

Use: 
- `docker-compose up` for local dev
- `docker-compose up` for CI env
- `docker stack deploy` for prod env

## Example 

Our file structure may look like this:
- docker-compose.yml
    - contains only the basics and common things for all env
- docker-compose.override.yml
    - this file overrides normal docker-compose file in local-dev
    - .override.yml is a Docker convention to allow overriding settings
    - docker reads this file and applies the settings in it over and above the normal docker-compose file
    - below: notice image name is missing in this file, it comes from docker-compose
- docker-compose.test.yml
    - in prod and test, the override file must be specified manually
- docker-compose.prod.yml
    - in prod we will not have access to `docker-compose` command, so we use `docker-compose config` setup
    - the config command can combine/squish multiple docker-compose files
- Dockerfile
- psql-fake-password.txt
- themes/

Commands:
- For local dev
    - `docker-compose up`
    - picks up docker-compose + docker-compose.override
- for CI/test
    - make sure docker-compose is installed
    - `docker-compose -f docker-compose.yml -f docker-compose.test.yml up`
    - with `-f` flag, the base file goes first, followed by the overrides
- Prod
    - `docker-compose -f docker-compose.yml -f docker-compose.prod.yml config > output.yml`
    - the `output.yml` file is then used to run a swarm stack


docker-compose.yml
```yaml
version: '3.9'

services:

  drupal:
    image: custom-drupal:latest

  postgres:
    image: postgres:14.3
```

docker-compose.override.yml
```yaml
version: '3.9'

services:

  drupal:
    build: .
    ports:
      - "8080:80"
    volumes:
      - drupal-modules:/var/www/html/modules
      - drupal-profiles:/var/www/html/profiles
      - drupal-sites:/var/www/html/sites
      - ./themes:/var/www/html/themes
 
  postgres:
    environment:
      - POSTGRES_PASSWORD_FILE=/run/secrets/psql-pw
    secrets:
      - psql-pw
    volumes:
      - drupal-data:/var/lib/postgresql/data

volumes:
  drupal-data:
  drupal-modules:
  drupal-profiles:
  drupal-sites:
  drupal-themes:

secrets:
  psql-pw:
    file: psql-fake-password.txt
```

docker-compose.test.yml
```yaml
version: '3.9'

services:

  drupal:
    image: custom-drupal
    build: .
    ports:
      - "80:80"

  postgres:
    environment:
      - POSTGRES_PASSWORD_FILE=/run/secrets/psql-pw
    secrets:
      - psql-pw
    volumes:
      # NOTE: this might be sample data you host in your CI server
      # so you can do integration testing with sample data
      # this may not work on Docker for Windows/Mac due to bind-mounting
      # database data across OSes, which doesn't always work
      # in those cases you should use named volumes
      - ./sample-data:/var/lib/postgresql/data
secrets:
  psql-pw:
    file: psql-fake-password.txt

```

docker-compose.prod.yml
```yaml
version: '3.9'

services:

  drupal:
    ports:
      - "80:80"
    volumes:
      - drupal-modules:/var/www/html/modules
      - drupal-profiles:/var/www/html/profiles
      - drupal-sites:/var/www/html/sites
      - drupal-themes:/var/www/html/themes
 
  postgres:
    environment:
      - POSTGRES_PASSWORD_FILE=/run/secrets/psql-pw
    secrets:
      - psql-pw
    volumes:
      - drupal-data:/var/lib/postgresql/data

volumes:
  drupal-data:
  drupal-modules:
  drupal-profiles:
  drupal-sites:
  drupal-themes:

secrets:
  psql-pw:
    external: true
```

---

# Service Updates

We can provide rolling replacement of tasks/containers in a service

Will replace container for most changes

Most of the update commands will be the same, just add -add or -rm option to add/remove

Includes rollback and healthcheck options

Also has scale & rollback subcommands for quicker access
    - `docker service scale web=4`
    - `docker service rollback web`

Note: a stack deploy, when pre-existing/already running, will issue service updates (i.e. update the service)

##### Command Examples

- Just update the image used to a newer version (1.2.0 > 1.2.1)
    - `docker service update --image myapp:1.2.1 <service_name>`
- Adding an env variable and remove a port
    - `docker service update --env-add NODE_ENV=production --publish-rm 8080`
- Change number of replicas of two services
    - `docker service scale web=8 api=6`
- For swarm updates
    - Just update the YAML file and then run
    - `docker stack deploy -c file.yml <stack_name>`
- To force update a service
    - `docker service update --force <service_name>`
    - This will rebalance the tasks among nodes