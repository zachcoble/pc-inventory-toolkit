# PC Inventory Toolkit

Inventory a Windows or Linux PC well enough to **order the right part** — without
opening the case.

Two dependency-free scripts and a field guide for turning raw specs into a
correct order. Built around a practical question: what do I actually need to
know to buy a compatible replacement or upgrade?

---

## Quickstart

```powershell
git clone https://github.com/zachcoble/pc-inventory-toolkit.git
cd pc-inventory-toolkit

# Hardware inventory (run elevated for complete results)
.\scripts\Get-PCSpecs.ps1

# JSON, for pasting into a chat or piping elsewhere
.\scripts\Get-PCSpecs.ps1 -Json
```

Linux:

```bash
sudo ./scripts/get-pc-specs.sh
```

---

## What's here

| Path | What it is |
|---|---|
| `scripts/Get-PCSpecs.ps1` | Windows hardware inventory aimed at ordering parts |
| `scripts/get-pc-specs.sh` | Linux equivalent (dmidecode / lsblk / lspci) |
| `docs/ordering-parts.md` | Turning the script's output into a correct order |
| `docs/claude-code-local-setup.md` | Running Claude Code locally so it can see real hardware |
| `templates/claude-settings.json` | Claude Code permission config to copy into a project or `~/.claude/` |

---

## Requirements

- **Windows script:** PowerShell 5.1+ (ships with Windows 10/11). No modules to
  install. Run as Administrator for serial numbers and full disk detail.
- **Linux script:** bash, plus `dmidecode`, `pciutils`, `util-linux` for full
  output. Degrades gracefully without them.

---

## Also here: know which layer you're standing on

The scripts above have to run *on* the hardware you're asking about. That sounds
obvious until you try it from a cloud agent session.

A Claude Code session started from the web runs in an ephemeral Linux container
in Anthropic's cloud. It clones your repo and gives you real tools — but those
tools operate on the container, not on your desktop. Asking it about your CPU
gets you an honest answer about the wrong machine:

```
Model name:   Intel(R) Xeon(R) Processor @ 2.80GHz
CPU(s):       4
Mem:          15Gi
```

That's a hypervisor slice. Your PC is not involved.

It's the same mistake as running `lscpu` inside a VM, or `dmidecode` inside WSL,
and thinking you learned something about the host. The tool is telling the truth
about the layer it's standing on — which is the thing that catches people out
when they start doing virtualization and cloud work.

To inventory your own hardware you need the agent running on that hardware.
Install steps, permission modes (`default` → `acceptEdits` → `auto` →
`bypassPermissions`), and the settings-precedence gotcha are in
[`docs/claude-code-local-setup.md`](docs/claude-code-local-setup.md); a ready-made
permission config is in [`templates/claude-settings.json`](templates/claude-settings.json).
