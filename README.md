# ChatGPT Web Chat

A lightweight, responsive browser-based chat interface for an OpenAI-compatible Chat Completions API. The interface is designed for desktop and mobile browsers and includes model selection, local conversation history, JSON backup/restore, link previews, and optional PWA integration.

> **Important:** This project is a frontend HTML application. The API endpoint must support browser cross-origin requests (CORS), or requests must be routed through a backend proxy.

## Features

- **Chat with multiple models** through a configurable `/chat/completions` endpoint.
- **Model selection in Settings**, including the following model IDs, in the configured order:
  - `gpt-5.6-sol` — 5.6 Sol
  - `gpt-5.6-terra` — 5.6 Terra
  - `gpt-5.6-luna` — 5.6 Luna
  - `gpt-5.4` — 5.4
  - `gpt-5.4-mini` — 5.4 mini
  - `gpt-5.4-nano` — 5.4 nano
  - `gpt-5.2` — 5.2
  - `gpt-5.2-pro` — 5.2 Pro
  - `o4-mini` — o4-mini
  - `o3-pro` — o3-pro
  - `o3-mini` — o3-mini
  - `gpt-4.1` — 4.1
  - `gpt-4.1-mini` — 4.1 mini
  - `gpt-4o` — 4o
  - `gpt-4o-mini` — 4o mini
- **Configurable API settings:** API key, API Base URL, and selected model.
- **Local conversation persistence** using browser storage where supported.
- **New chat on page load**, while previous conversations remain available from the sidebar.
- **Sidebar search** for conversation titles and message text.
- **Long-press deletion** with a confirmation dialog.
- **JSON export and import** for conversation backups. Export includes all conversations; import merges by conversation ID.
- **Inline source-link pills:** Markdown links can be displayed as compact domain buttons.
- **Link preview bottom sheet:** Attempts to display site metadata and provides an option to open the original URL.
- **Optional PWA support** through a web app manifest and service worker.
- **Responsive layout** for mobile and desktop screens.

## Getting Started

1. Place the HTML file on a web server, preferably one served over HTTPS.
2. Open the page in a modern browser.
3. Open **Settings** using the `⌘` button.
4. Enter your API key.
5. Enter the API Base URL for your API service, for example:

   ```text
   https://your-api.example/v1
   ```

6. Choose a model and save the settings.
7. Start a new chat and send a message.

The application sends requests to:

```text
<API_BASE_URL>/chat/completions
```

The request uses a bearer token in the `Authorization` header and sends the selected model ID with the conversation messages.

## Keyboard Shortcuts

- **Enter:** Send the current message.
- **Shift + Enter:** Insert a newline in the message input.
- **Escape:** Close supported overlays or dialogs.

## Conversation Backup and Restore

Open **Settings** to access the JSON backup controls.

### Export

Exports all locally available conversations to a JSON file, not only the currently open conversation. Store the exported file somewhere safe as a backup.

### Import

Select a previously exported JSON file. Imported conversations are merged with the current history by conversation ID. If an imported conversation has the same ID as an existing conversation, the imported version replaces it.

Only import JSON files you trust.

## Link Previews

Markdown-style links such as:

```markdown
([medlineplus.gov](https://medlineplus.gov/ency/article/002208.htm?utm_source=openai))
```

can be represented by a compact domain button. Clicking the button opens a bottom-sheet preview with the website domain and, when available, page title and description. Users can open the original URL from the preview or dismiss the sheet by clicking the backdrop.

Metadata retrieval may fail for some sites because of CORS restrictions, unavailable metadata, network failures, or third-party service limitations. In that case, the preview displays a fallback rather than guaranteeing a complete page preview. A preview is metadata-based; it does not embed the full external page.

**Privacy note:** A third-party metadata service may receive the URL being previewed. Avoid previewing URLs containing private tokens or other sensitive information.

## PWA Setup

The HTML references the following files, which must be supplied separately:

- `manifest.json`
- `sw.js`
- `icon-192.png`

Ensure the paths in the HTML match where these files are deployed. The service worker registration uses a relative path (`./sw.js`), which is suitable for many subdirectory deployments.

PWA installation and service workers generally require **HTTPS** or `localhost`; opening the HTML directly with a `file://` URL is not sufficient for service worker registration. The manifest should include the appropriate application name, start URL, display mode, icons, and theme colors for your deployment.

## Storage and Security

- API settings and conversation history are stored in the browser where storage is available.
- Browser storage is specific to the browser profile and origin. Opening the page from a different origin or browser may show a separate history.
- Private browsing, browser data clearing, storage quotas, or browser policies can remove or block stored data. Export conversations regularly if they matter.
- Anyone with access to the same browser profile may be able to access locally stored settings. Avoid saving an API key on a shared device.
- For production use, consider a backend proxy to protect credentials and handle upstream API requests securely.
- Do not expose API keys in publicly shared HTML files or commit them to a public repository.

## Troubleshooting

### The settings dialog does not open

Use the latest HTML file and open it in a modern browser. If the page is embedded in a restricted HTML previewer, try serving it from a local development server or HTTPS site.

### API requests fail

Check that the API key, Base URL, and model ID are correct. Confirm that the API service supports the Chat Completions request format and allows browser CORS requests. If the service does not support CORS, use a backend proxy.

### Conversation history or settings are missing

Make sure you are using the same browser profile and origin. Check whether private browsing or browser storage restrictions are enabled. Restore a JSON backup if needed.

### A link preview has no title or description

The destination site or metadata provider may block access or return incomplete metadata. The original link can still be opened from the preview.

### PWA installation is unavailable

Serve the site over HTTPS (or localhost for development), verify that `manifest.json`, `sw.js`, and the icon are reachable at the configured paths, and inspect the browser's developer console for errors.

## Compatibility Notes

- The API service must accept OpenAI-compatible Chat Completions requests.
- Link previews depend on browser networking and the configured metadata retrieval service.
- The app's persistence behavior depends on browser storage support and policy.
- A service worker and manifest alone do not guarantee successful installation; valid deployment paths and manifest contents are also required.

## License

No license is specified in this README. Add a license file if you plan to distribute or publish the project.
