# unraid-apps

Xeek's favorite Unraid Docker templates and builds.

## Templates

| App | Description | Image |
|-----|-------------|-------|
| [Speaches](x3ek/speaches.xml) | Speech-to-text and text-to-speech server (GPU) | `ghcr.io/speaches-ai/speaches` |
| [Chatterbox TTS](x3ek/chatterbox-tts.xml) | Text-to-speech API with voice cloning (GPU) | `ghcr.io/x3ek/chatterbox-tts-api` |

## Usage

Copy a template XML to your Unraid server's template directory:

```bash
# From Unraid terminal
cd /boot/config/plugins/dockerMan/templates-user
wget https://raw.githubusercontent.com/x3ek/unraid-apps/main/x3ek/speaches.xml
wget https://raw.githubusercontent.com/x3ek/unraid-apps/main/x3ek/chatterbox-tts.xml
```

Then go to **Docker** > **Add Container** and select the template from the dropdown.

## Custom Builds

The Chatterbox TTS API image is built weekly from [travisvn/chatterbox-tts-api](https://github.com/travisvn/chatterbox-tts-api) with an additional Blackwell-compatible variant. Images are published to `ghcr.io/x3ek/chatterbox-tts-api`.

| Tag | Description |
|-----|-------------|
| `latest` | Standard CUDA build |
| `blackwell` | Blackwell GPU (RTX 50xx) compatible build |
