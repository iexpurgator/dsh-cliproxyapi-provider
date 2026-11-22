# CLIProxyAPI Provider for DeepSeek Harness

Adds a `CLIProxyAPI` model provider based on the OpenAI Responses API to DeepSeek Harness.

The plugin automatically retrieves the model list from CLIProxyAPI, so models do not need to be added or maintained manually.

## Usage

Install the plugin:

```powershell
npx @deepseek-ai/dsh plugin --profile web add github:iexpurgator/dsh-cliproxyapi-provider
```

Start or restart DeepSeek Harness Web:

```powershell
npx @deepseek-ai/dsh web
```

After opening Harness:

1. Open the sidebar **Plugins** page and select **CLIProxyAPI** from the Official plugins list.
2. Enter the CLIProxyAPI **Base URL**, for example
   `http://127.0.0.1:8317/v1`.
3. Enter the **API key**. Leave it empty if the service does not require authentication.
4. Save the configuration. The model list will be retrieved automatically and refreshed periodically. Models whose catalog `input_modalities` includes `image` automatically receive image-input support. If CLIProxyAPI omits this field, image support is treated as unknown rather than guessed.

Uninstall the plugin (use the package name, not the GitHub URL):

```powershell
npx @deepseek-ai/dsh plugin --profile web remove @iexpurgator/dsh-cliproxyapi-provider
```

Restart DeepSeek Harness Web after uninstalling the plugin.

## Thinking / reasoning levels

The plugin keeps each model's catalog capabilities in sync. If CLIProxyAPI does not
declare `supported_reasoning_levels`, DSH will not offer a thinking/effort selector
for that model. You can enable it manually in **Settings → Models → CLIProxyAPI**
by adding reasoning levels to the model; those edits are preserved across refreshes.

## Upgrading from older versions

Older installations were published as `@router-for-me/dsh-cliproxyapi-provider@0.1.0`
and are incompatible with DSH `0.2.2-rc.5`. Remove the old entry first, then install
the current version:

```powershell
npx @deepseek-ai/dsh plugin --profile web remove @router-for-me/dsh-cliproxyapi-provider
npx @deepseek-ai/dsh plugin --profile web add github:iexpurgator/dsh-cliproxyapi-provider
```

If DSH still reports the old version during removal, grant a temporary exemption and
retry:

```powershell
npx @deepseek-ai/dsh plugin --profile web allow-version @router-for-me/dsh-cliproxyapi-provider@0.1.0 --dsh-version 0.2.2-rc.5 --accept-risk
```

## Compatibility

This release targets DeepSeek Harness `0.2.2-rc.5`. It contributes an official plugin configuration page through `plugins.item`, edits the `llm-pi-ai` entry with `configForms`, and uses `ctx.remote` for credentials and model discovery. The page appears only while the Host serves the `llm-pi-ai` configuration entry; model discovery uses the `llm-pi-ai` namespace registered for this provider family. Restart the Web process after installing or upgrading so DSH loads the current client bundle.
