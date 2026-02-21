# Unraid Template Conventions

This repo contains Unraid Docker templates in binhex-style XML format. Follow these conventions when creating or modifying templates.

## XML Structure

```xml
<?xml version="1.0"?>
<Container version="2">
  <!-- Metadata -->
  <Name>x3ek-{appname}</Name>
  <Repository>{image}:{tag}</Repository>
  ...
  <!-- Config entries: Ports, then Paths, then Variables -->
  <Config ...>value</Config>
</Container>
```

## Config Attribute Ordering

Always use this attribute order on `<Config>` elements:

```
Name, Target, Default, Mode, Description, Type, Display, Required, Mask
```

## Config Name Prefixes

- Ports: `Name="Port: {description}"`
- Paths: `Name="Path: {description}"`
- Variables: `Name="Variable: {description}"`

## Section Ordering

Config entries must appear in this order:
1. **Ports** (Type="Port")
2. **Paths** (Type="Path")
3. **Variables** (Type="Variable")

Within each section, "always" display items come before "advanced" display items.

## Display Values

- `Display="always"` — shown on the basic config screen (commonly changed settings)
- `Display="advanced"` — hidden behind "Show more settings" (rarely changed)

## Container Naming

Container names follow the pattern: `x3ek-{appname}` (e.g., `x3ek-speaches`, `x3ek-chatterbox-tts`)

## GPU Containers

For containers that use NVIDIA GPUs:

1. Set `<ExtraParams>--gpus all</ExtraParams>` (user can change to `--gpus "device=0"` etc.)
2. Add `<Requires>` mentioning NVIDIA GPU and the Unraid NVIDIA plugin
3. Include these as **advanced** variables:
   - `NVIDIA_VISIBLE_DEVICES` = `all`
   - `NVIDIA_DRIVER_CAPABILITIES` = `compute,utility`

## Generic Defaults

Templates must use **generic defaults**, not user-specific values:
- Paths: `/mnt/user/appdata/{appname}/...`
- Timezone: `America/New_York` (common default, user changes to their own)
- Ports: use the application's default port

## Template URL

Every template should include:
```xml
<TemplateURL>https://raw.githubusercontent.com/x3ek/unraid-apps/main/x3ek/{appname}.xml</TemplateURL>
```

## Inspecting Running Containers

To extract config from a running Unraid container for template creation:

```bash
# Get full container config
docker inspect {container_name}

# Key fields to extract:
# - Image / repository tag
# - Ports: .NetworkSettings.Ports or .HostConfig.PortBindings
# - Volumes: .Mounts
# - Environment: .Config.Env
# - GPU config: .HostConfig.DeviceRequests
# - Extra params: .HostConfig (various fields)
```

## File Placement

- Templates go in: `x3ek/{appname}.xml`
- Icons go in: `images/{appname}.png` (256x256 recommended)
- Build workflows go in: `.github/workflows/build-{appname}.yml`
