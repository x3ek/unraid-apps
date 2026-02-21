# unraid-apps

Xeek's favorite Unraid Docker templates and builds.

## Templates

| App | Description | Image |
|-----|-------------|-------|
| [Speaches](x3ek/speaches.xml) | Speech-to-text and text-to-speech server (GPU) | `ghcr.io/speaches-ai/speaches` |
| [Chatterbox TTS](x3ek/chatterbox-tts.xml) | Text-to-speech API with voice cloning (GPU) | `ghcr.io/x3ek/chatterbox-tts-api` |

## Usage

Add this repo as a template repository in Unraid:

1. Go to the **Docker** tab
2. Scroll to the bottom and add this URL to the template repositories field: `https://github.com/x3ek/unraid-apps`
3. Click **Save**

Templates will appear in the **Add Container** template dropdown.

## Custom Builds

The Chatterbox TTS API image is built weekly from [travisvn/chatterbox-tts-api](https://github.com/travisvn/chatterbox-tts-api) with an additional Blackwell-compatible variant. Images are published to `ghcr.io/x3ek/chatterbox-tts-api`.

| Tag | Description |
|-----|-------------|
| `latest` | Standard CUDA build |
| `blackwell` | Blackwell GPU (RTX 50xx) compatible build |
