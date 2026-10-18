# ZeroScript ChatGPT Fix

Personal backup of ZeroScript 1.5.5 with a ChatGPT composer compatibility patch.
Includes the extension, local bridge, launchers, upstream documentation, and
GPL license.

**Deleted your local ZeroScript files? Start with [RECOVER-WINDOWS.md](RECOVER-WINDOWS.md).**
It covers downloading this private repository and reinstalling everything.

## Quick restore

1. Sign in to the GitHub account that owns this private repository.
2. Download [the latest ZIP](https://github.com/saliljena16-sys/zeroscript-chatgpt-fix/archive/refs/heads/main.zip),
   or click **Code > Download ZIP**. Extract the entire ZIP to a stable folder.
3. In Edge (`edge://extensions`) or Chrome (`chrome://extensions`), enable
   **Developer mode**, click **Load unpacked**, and select the extracted
   `zeroscript-extension` folder containing `manifest.json`.
4. Open a place in Roblox Studio. Enable **Assistant AI > ... > Manage MCP
   Servers > Enable Studio as MCP Server**.
5. Double-click `start.bat` in the extracted main folder. Leave its bridge
   window open. Python is required; the upstream Windows launcher attempts to
   install it when it is missing.
6. Open [ChatGPT](https://chatgpt.com/), hard-refresh with **Ctrl+Shift+R**,
   and look for ZeroScript above the input box.

The bridge uses the default Roblox MCP configuration when `config.json` is
absent. `config.example.json` records that default. Personal runtime configuration
and logs are intentionally ignored by Git.

## What changed

The October 2026 ChatGPT editor no longer carries `id="prompt-textarea"`.
ZeroScript's content script still loaded and connected to Studio, but its editor
lookup failed, so the UI code hid the toolbar. The observed native Send button
also lost its old id and test id.

Only `zeroscript-extension/providers/chatgpt.js` changes provider behavior:

- Find the visible ProseMirror editor through semantic composer attributes.
- Keep finding it while ZeroScript sets `contenteditable="false"` to lock input.
- Recognize the observed English Send button.
- Recognize the current input wrapper when covering a locked editor.
- Use the existing floating toolbar fallback when the old inline composer grid
  is absent.

The exact change is in [patches/chatgpt-composer.patch](patches/chatgpt-composer.patch).
The loaded provider stamps `data-zs-gpt-ver="2026-10-18_composer-selectors"` on
the page's `<html>` element.

## Validation

The new editor and Send selectors were checked on a live Edge ChatGPT page.
An unsent draft exposed the Send button and was cleared. The patched provider
passed JavaScript syntax checking and all 45 upstream checks (8 ChatGPT reader
checks and 37 parser checks).

```text
npm test
```

Node is only needed for these developer checks, not for the normal bridge
startup. No npm packages are required. The root package marks CommonJS so the
upstream test files run correctly even beneath a parent project using ES modules.

The extension reload, post-reload toolbar placement, complete agent session,
and native Stop/generation behavior have not been verified end to end. The new
Send fallback is for the observed English interface. See the recovery guide
for verification steps and troubleshooting.

## Attribution and license

Based on [sebattfg/ZeroScript-Free](https://github.com/sebattfg/ZeroScript-Free),
by ZeroDev. This is an independent personal backup and compatibility patch,
not an official upstream release. The source snapshot was copied from a local
ZeroScript 1.5.5 installation on October 18, 2026; upstream Git history is not
included. Original license notices remain intact. See [LICENSE](LICENSE) and
[README.upstream.md](README.upstream.md).

Related upstream reports: [#85](https://github.com/sebattfg/ZeroScript-Free/issues/85),
[#100](https://github.com/sebattfg/ZeroScript-Free/issues/100), and
[#98](https://github.com/sebattfg/ZeroScript-Free/issues/98).
