**English** · [Français](README.fr.md)

# LMClibot

**Your desktop AI agent, driving Claude Code or GitHub Copilot CLI for you.**

LMClibot is a small robot that floats above your Windows desktop. You give it a task in its bubble: it has
the command-line agent you already use (Claude Code or GitHub Copilot CLI) carry it out, shows you the answer
as it comes, and stops you when a command deserves your say before it runs. All the power of the agent,
without the terminal.

> This repository only holds the **compiled releases** (*Releases* tab). LMClibot is free, but its source
> code is not published.

## What LMClibot lets you do

- **Talk to your agent from a bubble.** Live answers, the tools being used, screenshots and files to attach,
  a message queue while the agent works, a Stop button.
- **Keep several sessions.** The Sessions window lists your conversations, finds them by search and resumes
  them where you left off. Each robot has its own color; "New Orchestrator" opens one more robot to run two
  jobs side by side.
- **Agent mode.** A long task is split into sectors handed to sub-agents (mini-robots appear around the
  head), then reviewed before it comes back to you.
- **Choose what the agent sees.** Menus for sub-agents, MCP servers, skills and the model, right in the
  bubble. Every unchecked box is a real restriction for the agent.
- **Auto mode, with a guard.** The agent keeps going without asking, but dangerous commands (recursive
  deletes, `git push --force`, `git reset --hard`, disk formatting, `DROP TABLE`…) are blocked and brought
  to you.
- **Presentation mode.** The agent answers with HTML slides, opened in a viewer, with a final questionnaire
  to send your decisions back.
- **Light, dark or Windows theme**, chosen at setup and changeable in Settings.
- **English or French interface**, following Windows by default, changeable in Settings. Everything
  LMClibot shows or sends to the agent follows that choice.

## Claude Code and GitHub Copilot CLI

LMClibot has no intelligence of its own and asks for no API key: it drives an agent already installed and
signed in on your machine. At first launch it detects both; you then pick the engine of each **new** session
in the Sessions window ("Engine" button). A session always keeps its engine.

| | Claude Code | GitHub Copilot CLI |
|---|---|---|
| Bubble, sessions, resume, Stop | yes | yes |
| Agent mode and sub-agents | yes | yes |
| MCP servers added in LMClibot | yes | yes |
| Skills | yes | yes: its own, plus your checked Claude skills |
| Auto mode with guard | yes | yes |
| Presentation mode | yes | yes |
| Dispatch to your phone, plugin catalogs, claude.ai connectors | yes | no (specific to Claude Code) |

## Requirements

- Windows 10 or 11 (64-bit). The installer adds WebView2 if it is missing.
- At least one of the two agents, installed and signed in to your account:
  - [Claude Code](https://docs.anthropic.com/claude-code) (Anthropic account);
  - [GitHub Copilot CLI](https://docs.github.com/copilot): `npm install -g @github/copilot`, then
    `copilot login` (any Copilot plan, the free one included, is enough).

## Install

1. Download `LMClibot-<version>-setup.exe` from the latest *Release* (installs without administrator
   rights), or `LMClibot-<version>-portable.exe` to run it without installing.
   Also download `SHA256SUMS.txt`, and `SHA256SUMS.txt.sig` if you want to check the signature.
2. Check the file (see below).
3. The binary is not signed with a code-signing certificate yet: Windows SmartScreen may show "Windows
   protected your PC". Click *More info › Run anyway* if the check passed.

## Check your download

### 1. The checksum (the file is intact)

In PowerShell, in the folder where you downloaded the files:

```powershell
$f = "LMClibot-1.1.1-setup.exe"   # the downloaded file
$expected = (Get-Content SHA256SUMS.txt | Where-Object { $_ -like "*  $f" }).Split(" ")[0]
(Get-FileHash $f -Algorithm SHA256).Hash -eq $expected
```

`True`: the file is exactly the one that was published. `False`: do not run it, download it again.

### 2. The signature (it really comes from the author)

The checksum proves the file is intact, not who published it. `SHA256SUMS.txt.sig` is an Ed25519 signature
of the version and of `SHA256SUMS.txt`, made with LMClibot's publishing key. Checking it needs OpenSSL 3
(shipped with [Git for Windows](https://git-scm.com/download/win)):

```powershell
$v = "1.1.1"   # the downloaded version, without the "v"
"-----BEGIN PUBLIC KEY-----`nMCowBQYDK2VwAyEA/ocxSvM6vVUpPxIejFpi1EAH2dZd8srOz8jTXK95+Bo=`n-----END PUBLIC KEY-----" |
  Set-Content -Encoding ascii lmclibot-key.pem
[byte[]]$m = [Text.Encoding]::ASCII.GetBytes("lmclibot $v`n") + [IO.File]::ReadAllBytes("$PWD\SHA256SUMS.txt")
[IO.File]::WriteAllBytes("$PWD\message.bin", $m)
[IO.File]::WriteAllBytes("$PWD\signature.bin", [Convert]::FromBase64String((Get-Content SHA256SUMS.txt.sig -Raw).Trim()))
& "C:\Program Files\Git\usr\bin\openssl.exe" pkeyutl -verify -pubin -inkey lmclibot-key.pem -rawin -in message.bin -sigfile signature.bin
```

`Signature Verified Successfully`: the checksums really come from the author, for this version. Then do
step 1: together, the two prove your file is genuine.

## Updates

LMClibot updates itself: it checks the versions published here at startup and then every 24 h, downloads
the new one in the background and installs it when you close the app. The **About** window (robot menu or
the icon near the clock) shows your version, offers "Update now" and lets you turn automatic updates off.

The app runs both checks above itself before any update, with the publishing key it carries: a version
whose signature does not match is refused.

## Security

Every release is built from source by continuous integration, then scanned by Trivy (dependencies, binary,
secrets) and govulncheck (Go standard library). No release ships with a fixable HIGH or CRITICAL
vulnerability. The software bill of materials (CycloneDX SBOM) is attached to every release.

LMClibot runs locally and sends no data to its author. Your messages only go through the agent you chose.
Only the update check contacts github.com (can be turned off).

## License and notices

Freeware, all rights reserved: see `LICENCE.txt` in each release. Third-party components:
`THIRD-PARTY-NOTICES.txt`.

LMClibot is an independent tool, **not affiliated with Anthropic or GitHub**, nor endorsed by them.
"Claude" and "Claude Code" are trademarks of Anthropic, PBC. "GitHub" and "GitHub Copilot" are trademarks
of GitHub, Inc.
