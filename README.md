
:bulb: <u>Starts phpMyAdmin in background on port 8080, connect to a MySQL container named 'db' via 'app_network' Docker network.</u>

```bash
docker run -d --rm --name phpmyadmin --network app_network -p 8080:80 -e PMA_HOST=db phpmyadmin/phpmyadmin
```

<hr>

:bulb: <u>Starts Mailpit in a detached container, maps ports 8025 (web UI) and 1025 (SMTP), and removes the container when stopped:</u>

```bash
docker run -d --rm -p 8025:8025 -p 1025:1025 --network app_network --name mailpit axllent/mailpit
```

```bash
MAIL_MAILER=smtp
MAIL_HOST=mailpit
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="no-reply@localhost"
MAIL_FROM_NAME="${APP_NAME}"
```

<hr>

:bulb: <u>Opens an interactive shell inside the running PHP container:</u>

```bash
docker compose exec php sh
```

<hr>

:bulb: <u>Creates a new Laravel project in the 'example-app' directory:</u>

```bash
composer create-project laravel/laravel example-app
```

<hr>

:bulb: <u>Starts a temporary Node container with port 5173 exposed and opens an interactive shell:</u>

```bash
docker compose run --rm -p 5173:5173 node sh
```

<hr>

:bulb: <u>Starts a temporary Node container, mounts the current directory to /app inside the container, sets the working directory to /app, uses the current user's UID and GID to avoid permission issues, and maps port 5173 for Vite development server access:</u>

```bash
docker run --rm -it -v "$(pwd)":/app -w /app -u "$(id -u):$(id -g)" -p 5173:5173 node bash
```

<hr>

:bulb: <u>Creates a new Vite project in the current directory using the latest version of Vite:</u>

```bash
npm create vite@latest .
```

<hr>

:bulb: <u>Runs the Vite development server and makes it accessible on the local network:</u>

```bash
npm run dev -- --host
```

<hr>

:bulb: <u>Configure in vite.config.js. Allows external access to the application and defines the port where Vite will run:</u>

```javascript
import { defineConfig } from 'vite'; // Imports the configuration function from Vite
import laravel from 'laravel-vite-plugin'; // Imports the official Laravel plugin for Vite
import tailwindcss from '@tailwindcss/vite'; // Imports the Tailwind CSS plugin for Vite

export default defineConfig({
  plugins: [
    laravel({
      input: ['resources/css/app.css', 'resources/js/app.js'], // Main CSS and JS files to be processed
      refresh: true, // Enables automatic refresh when saving Blade, CSS, or JS files
    }),
    tailwindcss(),
  ],
  server: {
    host: '0.0.0.0', // Allows connections from any IP (required in Docker)
    port: 5173, // Sets the port used by the Vite development server
    hmr: {
      host: 'localhost', // Address used by the browser to connect to the WebSocket (HMR)
      port: 5173, // Port used for Hot Module Replacement (HMR)
    },
    watch: {
      usePolling: true, // Forces file checking at intervals (avoids issues in Docker/WSL)
    },
  },
});
```

<hr>
