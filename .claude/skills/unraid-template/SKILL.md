---
name: unraid-template
description: Generate an Unraid XML template for a Docker container. Use when creating new Unraid app templates from an image name, running container, or app name.
argument-hint: "[app-name or image or container-name]"
---

Generate an Unraid XML template for `$ARGUMENTS`.

## Gathering Information

If the argument looks like a **running container name**, SSH to Unraid and inspect it:

```bash
ssh unraid "docker inspect $0"
```

Extract: image/tag, port bindings, volume mounts, environment variables, GPU device requests, extra host config.

If it's a **Docker image reference**, look up the project repo for a `docker-compose.yml`, `README`, or Dockerfile to find exposed ports, volumes, and environment variables.

If it's just an **app name**, search for the official Docker image and gather info from there.

## Generating the Template

Follow the conventions in `CLAUDE.md` exactly:

- Container name: `x3ek-{appname}`
- Config attribute ordering: Name, Target, Default, Mode, Description, Type, Display, Required, Mask
- Config section ordering: Ports → Paths → Variables
- Display="always" for commonly changed settings, Display="advanced" for rarely changed
- Generic defaults only — no user-specific paths, IPs, or keys
- Paths default to `/mnt/user/appdata/{appname}/...`

## GPU Containers

If the container uses NVIDIA GPUs (detected via DeviceRequests, NVIDIA env vars, or CUDA in image name):

- Set `<ExtraParams>--gpus all</ExtraParams>`
- Add `<Requires>` noting NVIDIA GPU + Unraid NVIDIA plugin

## Custom Build Workflow

If the app needs a custom Docker image build (no official image, or need a modified build):

- Create `.github/workflows/build-{appname}.yml`
- Include smart skip logic (compare upstream SHA against `org.opencontainers.image.revision` label)
- Use matrix builds for multiple tags if needed
- Push to `ghcr.io/x3ek/{appname}`

## Output

1. Write the template to `x3ek/{appname}.xml`
2. If a workflow was generated, write to `.github/workflows/build-{appname}.yml`
3. Validate the XML is well-formed using Python: `python -c "import xml.etree.ElementTree as ET; ET.parse('x3ek/{appname}.xml')"`
4. Update `README.md` to add the new template to the table
