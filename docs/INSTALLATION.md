# Installation Guide

Historical installation examples for the October 2025 n8n-skills snapshot.

## Setup status and boundaries

**NOT RUN / compatibility unverified.** This repository ships Markdown skills,
plugin metadata and evaluation scenarios, not a native application. It has no
package manifest, dependency lockfile, build command or automated evaluation
runner. The npm commands below concern the **external n8n-mcp server**, not a
repository build. No install, build, host session, MCP call or n8n API call was
performed during the documentation cleanup.

The instructions below are retained upstream examples, not confirmed current
platform support. Check current [Claude documentation](https://docs.anthropic.com/),
[n8n-mcp documentation](https://github.com/czlonkowski/n8n-mcp) and
[n8n documentation](https://docs.n8n.io/) for your versions before execution.
The clone/plugin examples select upstream, not necessarily this snapshot; pin
and review the desired revision before use. Automatic activation, upload UI,
plan requirements and custom-directory support are host/version dependent.

Inferred integration: a compatible host loads the seven skill directories and
separately connects to n8n-mcp. Discovery/validation/template capabilities may be
available without n8n credentials; instance workflow management requires a
separately configured n8n instance and permission. See the
[conceptual diagram](skill-context.svg) ([editable source](skill-context.excalidraw)).

Use placeholders only in shared configuration. Keep real credentials and workflow
or customer data outside tracked docs, use least-privilege sandbox access, and
avoid write-capable workflow examples against production. `.gitignore` excluding
`.mcp.json` does not establish secret absence or authorize publishing it.

---

## Prerequisites

### 1. n8n-mcp MCP Server

You **must** have the n8n-mcp MCP server installed and configured before using these skills.

**Install n8n-mcp**:
```bash
npm install -g n8n-mcp
```

**Configure MCP server** in `.mcp.json`:
```json
{
  "mcpServers": {
    "n8n-mcp": {
      "command": "npx",
      "args": ["n8n-mcp"],
      "env": {
        "MCP_MODE": "stdio",
        "LOG_LEVEL": "error",
        "DISABLE_CONSOLE_OUTPUT": "true",
        "N8N_API_URL": "https://your-n8n-instance.com",
        "N8N_API_KEY": "your-api-key-here"
      }
    }
  }
}
```

**Note**: `N8N_API_URL` and `N8N_API_KEY` are optional but enable workflow creation/management tools.

### 2. Claude Access

You need one of:
- **Claude Code** (compatible host version; interface not verified here)
- **Claude.ai** (web interface)
- **Claude API** (via SDK)

---

## Installation Methods

### Method 1: Claude Code (Recommended)

**Step 1**: Clone the repository
```bash
git clone https://github.com/czlonkowski/n8n-skills.git
cd n8n-skills
```

**Step 2**: Copy skills to Claude Code skills directory

**macOS/Linux**:
```bash
mkdir -p ~/.claude/skills
cp -r skills/* ~/.claude/skills/
```

**Windows**:
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills"
Copy-Item -Recurse skills\* "$env:USERPROFILE\.claude\skills\"
```

**Step 3**: Verify installation
```bash
ls ~/.claude/skills/
# Should show: n8n-expression-syntax, n8n-mcp-tools-expert, etc.
```

**Step 4**: Reload Claude Code
- Restart Claude Code application
- Skills will activate automatically

---

### Method 2: Claude.ai (Web Interface)

**Step 1**: Download skill folders

Download the repository and navigate to `skills/` directory. You'll need to upload each skill individually.

**Step 2**: Zip each skill
```bash
cd skills
zip -r n8n-expression-syntax.zip n8n-expression-syntax/
zip -r n8n-mcp-tools-expert.zip n8n-mcp-tools-expert/
zip -r n8n-workflow-patterns.zip n8n-workflow-patterns/
zip -r n8n-validation-expert.zip n8n-validation-expert/
zip -r n8n-node-configuration.zip n8n-node-configuration/
zip -r n8n-code-javascript.zip n8n-code-javascript/
zip -r n8n-code-python.zip n8n-code-python/
```

These historical repackaging examples do not include a license automatically.
Ensure the root MIT copyright and permission notice accompanies each substantial
copy, and retain skill credits and any applicable nested notices. The tracked
`dist/` directory contains only five individual ZIPs; the bundle has all seven.

**Step 3**: Upload to Claude.ai

1. Go to Claude.ai
2. Navigate to **Settings** → **Capabilities** → **Skills**
3. Click **Upload Skill**
4. Upload each `.zip` file individually
5. Confirm each upload

**Step 4**: Verify skills are active

In a new conversation, type:
```
"List my active skills"
```

If supported and all seven were installed, verify the seven source skills are available; this prompt is not proof of activation.

---

### Method 3: Claude API / SDK

**Historical illustrative code, not a verified SDK integration.** The loader
below is repository example code. In particular, `skills: skills` on
`client.messages.create` is not established as a supported SDK request parameter
by this repository. Do not treat it as copy-and-run setup; consult current host/API
documentation and implement its supported loading mechanism separately.

**Step 1**: Install via package manager

If you're building an application with Claude SDK:

```typescript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic({
  apiKey: process.env.ANTHROPIC_API_KEY,
});

// Load skills from directory
const skillsDir = './skills';
const skills = loadSkillsFromDirectory(skillsDir);

const response = await client.messages.create({
  model: 'claude-sonnet-4-5-20250929',
  messages: [{
    role: 'user',
    content: 'Build a webhook to Slack workflow'
  }],
  skills: skills // Pass loaded skills
});
```

**Step 2**: Skill loading function

```typescript
import fs from 'fs';
import path from 'path';

function loadSkillsFromDirectory(dir: string) {
  const skillDirs = fs.readdirSync(dir);
  return skillDirs.map(skillName => {
    const skillPath = path.join(dir, skillName, 'SKILL.md');
    const skillContent = fs.readFileSync(skillPath, 'utf-8');

    return {
      name: skillName,
      content: skillContent
    };
  });
}
```

---

## Verification

### Test Installation

**1. Check MCP server availability**
```
Ask Claude: "Can you search for the webhook node using n8n-mcp?"
```

Expected response:
```
[Uses search_nodes tool]
Found: nodes-base.webhook (Webhook trigger node)
```

**2. Test skill activation**
```
Ask Claude: "How do I access webhook data in n8n expressions?"
```

Expected response:
```
[n8n Expression Syntax skill activates]
Webhook data is under $json.body...
```

**3. Test cross-skill composition**
```
Ask Claude: "Build and validate a webhook to Slack workflow"
```

Historical expectation: relevant skills compose. Do not assume all seven activate for every query. This write-capable scenario needs an authorized sandbox.

---

## Troubleshooting

### Skills Not Activating

**Problem**: Skills don't activate when expected

**Solutions**:
1. Verify skills are in correct directory:
   - Claude Code: `~/.claude/skills/`
   - Check each skill has `SKILL.md` with frontmatter

2. Check SKILL.md frontmatter format:
   ```markdown
   ---
   name: n8n Expression Syntax
   description: Validate n8n expression syntax...
   ---
   ```

3. Reload Claude Code or clear cache

### MCP Tools Not Available

**Problem**: "n8n-mcp tools not available"

**Solutions**:
1. Verify `.mcp.json` is in correct location
2. Check n8n-mcp is installed: `npm list -g n8n-mcp`
3. Test MCP server: `npx n8n-mcp`
4. Restart Claude Code

### N8N API Tools Missing

**Problem**: "n8n_create_workflow not available"

**Solutions**:
1. Verify `N8N_API_URL` and `N8N_API_KEY` in `.mcp.json`
2. Test API access: `curl -H "X-N8N-API-KEY: your-key" https://your-n8n-instance/api/v1/workflows`
3. Skills will still work with read-only tools (search, validate, templates)

### Permission Issues

**Problem**: Cannot write to skills directory

Inspect the specific destination and its ownership first. Use a user-owned
skills directory supported by the host; do not apply recursive ownership or
permission changes to the entire host configuration, or elevate privileges just
to follow these historical examples.

---

## Uninstallation

### Remove All Skills

**Claude Code**:
```bash
rm -rf ~/.claude/skills/n8n-*
```

**Claude.ai**:
1. Go to Settings → Capabilities → Skills
2. Delete each n8n skill individually

### Remove Specific Skill

```bash
rm -rf ~/.claude/skills/n8n-expression-syntax
```

---

## Updating

### Update All Skills

```bash
cd n8n-skills
git pull origin main
cp -r skills/* ~/.claude/skills/
```

### Update Single Skill

```bash
cp -r skills/n8n-expression-syntax ~/.claude/skills/
```

---

## Advanced Configuration

### Custom Skill Location

If using custom skills directory:

```bash
# Set environment variable
export CLAUDE_SKILLS_DIR="/path/to/custom/skills"

# Copy skills
cp -r skills/* $CLAUDE_SKILLS_DIR/
```

### Selective Installation

Install only specific skills:

```bash
# Only expression syntax and MCP tools expert
cp -r skills/n8n-expression-syntax ~/.claude/skills/
cp -r skills/n8n-mcp-tools-expert ~/.claude/skills/
```

---

## Next Steps

For historical usage examples, see [USAGE.md](USAGE.md). Installation and runtime behavior remain unverified by this cleanup.

---

## Support

- **Issues**: https://github.com/czlonkowski/n8n-skills/issues
- **Discussions**: https://github.com/czlonkowski/n8n-skills/discussions
- **n8n-mcp**: https://github.com/romualdczlonkowski/n8n-mcp

---

Conceived by Romuald Członkowski - [www.aiadvisors.pl/en](https://www.aiadvisors.pl/en)
