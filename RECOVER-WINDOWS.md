# Restore ZeroScript after deleting everything

Bookmark this private repository now:

**https://github.com/saliljena16-sys/zeroscript-chatgpt-fix**

The repository contains this guide, the patched browser extension, and the
bridge/launcher files. Deleting your local files does not delete this GitHub
backup. You do not need Git, a terminal, Node, or an API key for these steps.

## 1. Download a fresh copy

1. Sign in to GitHub as **saliljena16-sys** (or an account granted access).
2. Open the repository link above. If it shows **404**, first check that you
   are signed into the correct account: the repository is private.
3. Click **Code**, then **Download ZIP**. Alternatively, while signed in,
   [download the ZIP directly](https://github.com/saliljena16-sys/zeroscript-chatgpt-fix/archive/refs/heads/main.zip).
4. Right-click the ZIP in Windows File Explorer and choose **Extract All**.
5. Keep the extracted project in a stable location such as
   `%USERPROFILE%\Documents\ZeroScript`. Open the extracted folder that contains
   `start.bat`, `bridge.py`, and `zeroscript-extension`.

**Extract the whole ZIP, not just start.bat. Keep the extracted folder:** an
unpacked browser extension loads its files from that folder every time.

## 2. Restore the browser extension

Use Chrome or Edge. Install either browser first if necessary, then:

1. Open `edge://extensions` in Edge or `chrome://extensions` in Chrome.
2. Turn on **Developer mode**.
3. If an old ZeroScript copy remains listed, turn it off before loading a fresh
   copy; two active copies can interfere. This does not require removing your
   other browser extensions.
4. Click **Load unpacked**.
5. Select the project's **zeroscript-extension** folder. This is the folder
   containing `manifest.json`, `background.js`, `core`, and `providers`.
   Do not select the ZIP, the outer project folder, or `providers`.
6. Check that **ZeroScript Free × Roblox Studio Agent** is enabled. Its version
   remains **1.5.5**, with this repository's compatibility patch applied.

If you later update the files in the same installed folder, click the
extension's **Reload** button and then hard-refresh any open ChatGPT page.

## 3. Restore Roblox Studio and its connection

1. If Roblox Studio was also deleted, reinstall it from Roblox's official
   Creator website, [create.roblox.com](https://create.roblox.com/).
2. Open Studio and open the place you want to work on.
3. Click **Assistant AI** in Studio's top bar.
4. Open the Assistant panel's **...** menu, then **Manage MCP Servers**.
5. Enable **Studio as MCP Server**. Upstream calls this option
   **Enable Studio as MCP Server**. Studio's menu wording may change in later
   releases; use its built-in MCP server setting if the exact label differs.

Restoring this tool does not restore deleted Roblox project files. Open your
existing saved or published place separately.

## 4. Start the bridge

1. In the extracted project's main folder, double-click **start.bat**.
2. Leave the bridge window open while using ZeroScript. Wait for it to finish
   starting and report a Studio connection.
3. If Python is missing, the upstream launcher attempts to install Python for
   you. If that fails, install Python 3.9 or later from
   [python.org/downloads/windows](https://www.python.org/downloads/windows/),
   enable **Add python.exe to PATH** in the installer when offered, and run
   `start.bat` again.

You do not have to recreate `config.json` for the default Roblox setup. The
included bridge supplies its default automatically when that file is missing.
`config.example.json` is a reference copy of the default. Any custom extra MCP
servers you previously configured must be set up again; private local settings
and logs are not part of this backup.

## 5. Open ChatGPT and check the toolbar

1. Open [chatgpt.com](https://chatgpt.com/) and sign in normally.
2. Open a new chat in normal **Chat** mode.
3. Press **Ctrl+Shift+R** to reload ChatGPT completely.
4. Look for the **ZeroScript** bar above the input box.
5. Check its connection dot:
   - **Green:** bridge and Studio are ready.
   - **Yellow:** bridge is running; open a Studio place and check its MCP setting.
   - **Grey:** bridge is offline; start `start.bat` and leave its window open.

The patched bar uses ZeroScript's existing floating placement above the editor
on the redesigned ChatGPT composer. It may not sit inside the rounded input.

## 6. Do a small check before editing your game

1. Type a short draft in ChatGPT without sending it. Check that the normal Send
   arrow appears, then clear the draft.
2. Open an expendable test place in Studio.
3. Click **Start Roblox agent** in the ZeroScript bar.
4. Ask: **“Read the place name and report it. Do not create, edit, or delete anything.”**
5. Confirm a reply arrives and the input unlocks. Check that **Stop** works during
   a response before using the agent for editing.

This repository's syntax and existing reader/parser checks passed. The live
editor and English Send selectors were verified with an unsent draft. A complete
agent/Stop test was not performed when this backup was prepared.

## If something is missing

| Symptom | What to check |
| --- | --- |
| GitHub says 404 | Sign in to the account that owns or can access the private repo. |
| Load unpacked reports no manifest | Select the inner `zeroscript-extension` folder after extracting the ZIP. |
| No bar on ChatGPT | Check that the extension is enabled and allowed on chatgpt.com, click Reload on the extension card, then Ctrl+Shift+R on ChatGPT. |
| An old ZeroScript version still runs | Check the installed source folder in the extension's Details page. You may have loaded another download. |
| Grey connection dot | Start `start.bat`; leave its window open. |
| Yellow connection dot | Open a place in Studio and enable Studio's MCP server. |
| start.bat says bridge.py is missing | Extract the entire archive; do not run start.bat inside the ZIP preview. |
| Bridge cannot start because its port is busy | Close another ZeroScript bridge window, then retry. |
| Bar returns but the agent stalls | The toolbar fix does not verify the entire agent flow. Note the error and check the upstream issues linked in README.md. |

Optional diagnostic: press **F12** on ChatGPT, open Console, and run this
read-only check:

```js
({
  build: document.documentElement.getAttribute('data-zs-gpt-ver'),
  barVisible: !!document.getElementById('zs-bar')?.getClientRects().length,
  bridge: document.getElementById('zs-dot')?.title
})
```

Expected build: `2026-10-18_composer-selectors`; expected `barVisible`: `true`.
An old build value means you are loading old files or need to refresh the page.

## Keep recovery easy

- Bookmark the repository and this guide. Your GitHub account access is needed
  to download a private repository.
- Keep the extracted project folder in a stable location.
- Next time you use ZeroScript, open Studio, run `start.bat`, then open ChatGPT.
- This restores the saved 1.5.5 snapshot and this patch. Future ChatGPT or Studio
  redesigns may require another compatibility update.
