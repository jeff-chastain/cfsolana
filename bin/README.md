# CFSolana Docker

## Prerequisites

- Docker or other Virtual Containerization software

### Podman

If you wish to use Podman, here are a few utilities to consider installing.

- aardvark-dns for resolution
- podman-compose compatibility program
- cockpit-podman for a Docker desktopesque dashboard.

## Install and Run

Note: if you are following along with Podman, ensure you also have the podman-compose compatibility  program available  and installed.

1. Rename the `.env.local` to `.env`
2. `docker-compose build|podman-compose build`
3. `docker-compose up -d|podman-compose up -d`
4. Open browser to `http://localhost:8080`
5. Install the ContentBox application through the UI wizard.
