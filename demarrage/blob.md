# blog

Here's a simple, production-ready docker-compose.yml for Ghost using SQLite (no separate DB needed for starters). Save it to a file, run docker compose up -d, then visit http://localhost:2368/ghost to set up the admin.

text
services:
  ghost:
    image: ghost:latest-alpine
    restart: always
    ports:
      - "2368:2368"
    environment:
      NODE_ENV: production
      url: http://localhost:2368  # Change to your domain
    volumes:
      - ghost_content:/var/lib/ghost/content

volumes:
  ghost_content:
Quick start steps
Save the YAML above as docker-compose.yml.

Run docker compose up -d (pulls images automatically).

Wait 1-2 minutes, then go to http://localhost:2368/ghost to create your admin account.

Add pages via the admin dashboard under Pages.

Upgrade to MySQL (optional)
For better performance with multiple users, replace the service with this (add a db service):

text
services:
  db:
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: your_root_password
      MYSQL_DATABASE: ghost
      MYSQL_USER: ghost
      MYSQL_PASSWORD: your_ghost_password
    volumes:
      - mysql_data:/var/lib/mysql

  ghost:
    image: ghost:latest-alpine
    restart: always
    depends_on:
      - db
    environment:
      database__client: mysql
      database__connection__host: db
      database__connection__user: ghost
      database__connection__password: your_ghost_password
      database__connection__database: ghost
      NODE_ENV: production
      url: http://localhost:2368
    volumes:
      - ghost_content:/var/lib/ghost/content
    ports:
      - "2368:2368"

volumes:
  mysql_data:
  ghost_content:
Run the same docker compose up -d command.

Pro tips
Domain/SSL: Add a reverse proxy like Traefik or Caddy for HTTPS (see official docs).

Backup: The volumes persist data; back up ./ghost_content regularly.

Updates: docker compose pull && docker compose up -d.

