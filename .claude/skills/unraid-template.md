# Skill: Generate Unraid Docker Template

Generate a binhex-style Unraid XML template for a Docker container.

## Inputs

Provide one or more of:
- **App name** — the human-readable name of the application
- **Docker image** — the full image reference (e.g., `ghcr.io/org/app:tag`)
- **Running container name** — name of a container running on Unraid to inspect

## Steps

### 1. Gather Container Information

If a **running container name** is provided, SSH to the Unraid server and inspect it:

```bash
ssh unraid "docker inspect {container_name}"
```

Extract:
- Image and tag
- Exposed ports and port bindings
- Volume mounts
- Environment variables
- GPU/device requests
- Any extra host config (restart policy, network mode, etc.)

If only an **image name** is provided, check for documentation:
- Look for a `docker-compose.yml` or `README` in the project repo
- Check Docker Hub / GHCR for the image's exposed ports and volumes

### 2. Generate the Template

Follow the conventions in `CLAUDE.md` exactly:

- Container name: `x3ek-{appname}`
- Attribute ordering: Name, Target, Default, Mode, Description, Type, Display, Required, Mask
- Section ordering: Ports → Paths → Variables
- Use "always" display for commonly changed settings, "advanced" for rarely changed
- GPU containers: `--gpus all` in ExtraParams, NVIDIA env vars in advanced
- Generic defaults only (no user-specific paths, IPs, or keys)

### 3. Handle GPU Containers

If the container uses NVIDIA GPUs (detected via DeviceRequests or NVIDIA env vars):

- Add `<ExtraParams>--gpus all</ExtraParams>`
- Add `<Requires>` noting NVIDIA GPU + Unraid plugin
- Add `NVIDIA_VISIBLE_DEVICES` and `NVIDIA_DRIVER_CAPABILITIES` as advanced variables

### 4. Generate Build Workflow (if needed)

If the app needs a custom Docker image build (e.g., no official image, or need a modified build):

- Create `.github/workflows/build-{appname}.yml`
- Include smart skip logic (compare upstream SHA against image label)
- Use matrix builds for multiple tags if needed
- Push to `ghcr.io/x3ek/{appname}`

### 5. Output

- Write the template to `x3ek/{appname}.xml`
- If a workflow was generated, write to `.github/workflows/build-{appname}.yml`
- Validate the XML is well-formed
- Update `README.md` to include the new template in the table

## Example Usage

> Generate an Unraid template for ollama using the image `ollama/ollama:latest`

> Create a template from my running container called `ollama`

> Build a template for `ghcr.io/open-webui/open-webui:cuda` with GPU support
