# unraid-apps

Xeek's favorite Unraid Docker templates and builds.

## Templates

| App | Description | Image |
|-----|-------------|-------|
| [Speaches](x3ek/speaches.xml) | GPU-accelerated speech-to-text and text-to-speech | `ghcr.io/speaches-ai/speaches` |
| [Chatterbox TTS](x3ek/chatterbox-tts.xml) | Chatterbox text-to-speech API (with Blackwell support) | `ghcr.io/x3ek/chatterbox-tts-api` |

## Usage

Add this repo as a template repository in Unraid:

1. Go to **Docker** tab in Unraid
2. Click **Template Repositories** at the bottom
3. Add: `https://github.com/x3ek/unraid-apps`
4. Click **Save**

Templates will appear in the **Add Container** dropdown.

## Custom Builds

The Chatterbox TTS API image is built weekly from [travisvn/chatterbox-tts-api](https://github.com/travisvn/chatterbox-tts-api) with an additional Blackwell-compatible variant. Images are published to `ghcr.io/x3ek/chatterbox-tts-api`.

| Tag | Description |
|-----|-------------|
| `latest` | Standard CUDA build |
| `blackwell` | Blackwell GPU (RTX 50xx) compatible build |
