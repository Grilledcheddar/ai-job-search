# OpenCode + YOLO Auto Setup

This repo was originally built around Claude Code. The OpenCode layer keeps the same workflow and adds:

- `opencode.json` for YOLO Auto as an OpenAI-compatible provider
- `AGENTS.md` for OpenCode project rules
- `.opencode/commands/*.md` wrappers for `/setup`, `/scrape`, `/rank`, `/apply`, `/interview`, `/outcome`, and `/upskill`

## 1. Install prerequisites on Windows

From an elevated PowerShell where needed:

```powershell
winget install Git.Git
winget install OpenJS.NodeJS.LTS
winget install Oven-sh.Bun
winget install MiKTeX.MiKTeX
```

Install OpenCode with npm:

```powershell
npm install -g opencode-ai
opencode --version
```

Optional ATS text extraction support:

```powershell
choco install poppler
```

If you do not use Chocolatey, skip Poppler for now. The workflow can still visually review PDFs, but ATS text-layer checks will be weaker.

## 2. Clone the repo

```powershell
git clone https://github.com/Grilledcheddar/ai-job-search.git
cd ai-job-search
```

If you already cloned it:

```powershell
cd C:\path\to\ai-job-search
git pull
```

## 3. Set your YOLO Auto API key

For the current PowerShell session:

```powershell
$env:YOLO_AUTO_API_KEY = "paste-your-key-here"
```

Persist it for future PowerShell sessions:

```powershell
setx YOLO_AUTO_API_KEY "paste-your-key-here"
```

Close and reopen the terminal after `setx`.

## 4. Install the Bun job-search CLI dependencies

Run from the repo root:

```powershell
$tools = @("jobbank-search", "jobdanmark-search", "jobindex-search", "jobnet-search", "linkedin-search")
foreach ($tool in $tools) {
  Push-Location ".agents/skills/$tool/cli"
  bun install
  Pop-Location
}
```

The Danish portal tools may not matter for a US/Oklahoma job search, but the LinkedIn tool and the CLI pattern are still useful. Use `/add-portal` later to scaffold additional portals.

## 5. Verify LaTeX tools

```powershell
lualatex --version
xelatex --version
```

Smoke-test the CV template:

```powershell
Push-Location cv
lualatex -interaction=nonstopmode -halt-on-error main_example.tex
Pop-Location
```

## 6. Start OpenCode

```powershell
opencode
```

OpenCode reads:

- `opencode.json` for the YOLO Auto provider/model
- `AGENTS.md` for project rules
- `CLAUDE.md` and `.claude/skills/**` through the `instructions` list in `opencode.json`
- `.opencode/commands/*.md` for slash-command wrappers

## 7. First commands to run

Inside OpenCode:

```text
/setup
```

Then either search or apply directly:

```text
/scrape
/rank
/apply https://example.com/job-posting
```

For a pasted job description:

```text
/apply paste the full job description here
```

## 8. OpenCode command mapping

| OpenCode command | Existing source workflow |
|---|---|
| `/setup` | `.claude/commands/setup.md` |
| `/scrape` | `.claude/commands/scrape.md` |
| `/rank` | `.claude/commands/rank.md` |
| `/apply` | `.claude/commands/apply.md` |
| `/interview` | `.claude/commands/interview.md` |
| `/outcome` | `.claude/commands/outcome.md` |
| `/upskill` | `.claude/commands/upskill.md` |

## 9. Troubleshooting

### OpenCode does not see YOLO Auto

Check that the environment variable exists:

```powershell
$env:YOLO_AUTO_API_KEY
```

Then restart OpenCode from the same terminal.

### Wrong model is selected

Run `/models` inside OpenCode and choose:

```text
yolo-auto/qwen3.6-35b-a3b
```

### Commands are missing

Confirm the files exist:

```powershell
Get-ChildItem .opencode\commands
```

Then restart OpenCode from the repo root.

### PDF compile fails

Open MiKTeX Console and allow package installation on demand, then rerun the failing `lualatex` or `xelatex` command.
