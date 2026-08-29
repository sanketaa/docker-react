# Dockerized React CI/CD Lab

A frontend DevOps learning project that demonstrates how to develop, test, containerize, and serve a React application with Docker, Docker Compose, Nginx, and Travis CI.

## Background

The application itself is intentionally small: a Create React App starter interface with a basic render test. The main focus of the repository is the delivery workflow surrounding the frontend application.

The project explores two container strategies:

- A development container that runs the React development server with live source mounting
- A multi-stage production image that builds static assets with Node.js and serves them through Nginx

A Travis CI configuration builds the development image and runs the React test suite inside a container.

This is a historical educational lab. Its React, Node.js, build-tool, CI, and container-image versions should be updated before reuse.

## Development Workflow

`Dockerfile.dev` creates the development environment:

1. Start from a Node.js Alpine image.
2. Set `/app` as the working directory.
3. Install dependencies from `package.json`.
4. Copy the application source.
5. Start the React development server.

`docker-compose.yml` exposes port `3000` and mounts the local source directory into the container. A separate anonymous volume preserves the container's `node_modules` directory.

## Production Build

The production `Dockerfile` uses a multi-stage build:

1. A Node.js builder stage installs dependencies and runs `npm run build`.
2. The generated static files are copied into an Nginx image.
3. Nginx serves the optimized frontend assets.

This pattern keeps build dependencies out of the final runtime image.

## Continuous Integration

The historical `.travis.yml` workflow:

1. Enables Docker in the CI environment.
2. Builds the development image.
3. Runs the React test suite inside the built container.
4. Requests coverage output from the test command.

## Technologies

- JavaScript
- React
- Create React App
- Docker
- Docker Compose
- Nginx
- Travis CI
- Node.js and npm
- Git

## Repository Structure

```text
.
├── .travis.yml
├── Dockerfile
├── Dockerfile.dev
├── docker-compose.yml
├── package.json
├── package-lock.json
├── public
└── src
    ├── App.js
    ├── App.test.js
    ├── index.js
    └── serviceWorker.js
```

## Skills Demonstrated

- Containerizing a frontend development environment
- Using bind mounts and container volumes for local development
- Creating a multi-stage production image
- Separating build-time and runtime dependencies
- Serving static frontend assets with Nginx
- Running automated tests inside a container
- Defining a container-based CI workflow
- Maintaining reproducible dependencies with a lockfile

## Run Locally

The committed dependencies are historical, so review and update them before running on a modern workstation.

Using npm:

```bash
npm install
npm test
npm start
```

Using Docker Compose:

```bash
docker compose up --build
```

The development application is exposed at:

```text
http://localhost:3000
```

Build the production image:

```bash
docker build -t docker-react .
docker run --rm -p 8080:80 docker-react
```

Then open `http://localhost:8080`.

## Security and Modernization Notes

Do not treat the historical configuration as production-ready.

- Update React, React DOM, React Scripts, Node.js, and transitive dependencies.
- Pin specific base-image versions instead of relying on mutable tags.
- Run current dependency and container vulnerability scans.
- Replace the retired Travis CI configuration or update it to current syntax.
- Add a dedicated Nginx configuration with security headers and caching rules.
- Run the Nginx container with an appropriate non-root configuration.
- Expand the single starter test into meaningful component and integration tests.
- Add linting, build verification, image scanning, and deployment controls.
- Use `npm ci` in automated builds for lockfile-based dependency installation.

## Portfolio Context

This project demonstrates practical understanding of frontend containerization, multi-stage Docker builds, development-versus-production environments, Nginx static hosting, and container-based continuous integration. It is presented as a transparent learning project rather than a production application.
