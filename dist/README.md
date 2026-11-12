# n8n-mcp Skills - Distribution Packages

This folder contains historical v1.0.0 distribution packages for different Claude platforms.

**Snapshot status:** source and the complete bundle contain seven skills; only
five prebuilt individual ZIPs are tracked. JavaScript/Python source folders can be
packaged separately, but no individual archives for them are supplied here.
Commands, platform UI and plan requirements below are inherited October 2025
examples, **NOT RUN** or verified against current hosts. Plugin commands target
upstream and may not install these archived bytes. The archives have not been
rebuilt by this documentation cleanup; their embedded docs remain historical.
See [setup boundaries](../docs/INSTALLATION.md#setup-status-and-boundaries).

## 📦 Available Packages

### Complete Bundle (Recommended)

- **`n8n-mcp-skills-v1.0.0.zip`** (168 KB) - All 7 skills in one package

**Includes:**
- Skill #1: n8n Expression Syntax
- Skill #2: n8n MCP Tools Expert
- Skill #3: n8n Workflow Patterns
- Skill #4: n8n Validation Expert
- Skill #5: n8n Node Configuration
- Skill #6: n8n Code JavaScript
- Skill #7: n8n Code Python

**Installation:**
```bash
# Claude Code plugin installation
/plugin install czlonkowski/n8n-skills

# Or install from local file
/plugin install /path/to/n8n-mcp-skills-v1.0.0.zip
```

### For Claude.ai Users (Individual Skills)

Upload each skill separately via Settings → Capabilities → Skills (bottom of page):

- `n8n-expression-syntax-v1.0.0.zip` - n8n expression syntax and common patterns
- `n8n-mcp-tools-expert-v1.0.0.zip` - Expert guide for using n8n-mcp tools (recommended to install first)
- `n8n-workflow-patterns-v1.0.0.zip` - 5 proven workflow architectural patterns
- `n8n-validation-expert-v1.0.0.zip` - Validation error interpretation and fixing
- `n8n-node-configuration-v1.0.0.zip` - Operation-aware node configuration

**Installation:**
1. Go to Settings → Capabilities → Skills (bottom of page)
2. Click "Upload Skill"
3. Select one of the skill zip files above
4. Repeat for each skill you want to install

**Note:** Prebuilt individual ZIPs for JavaScript and Python are absent; both skills exist in source and in the complete bundle.

## 🎯 Which Package Should I Use?

| Platform | Package | What You Get |
|----------|---------|--------------|
| **Claude.ai** | Individual zips | 5 core skills (upload separately) |
| **Claude Code** | Complete bundle (n8n-mcp-skills-v1.0.0.zip) | All 7 skills at once |
| **Claude API** | Complete bundle | All 7 skills (extract skills/ folder) |

**Note:** Among the tracked ZIPs, Code skills (#6 JavaScript, #7 Python) are only in the complete bundle; their source is also under `skills/`.

---

## 📁 Files in This Directory

```
dist/
├── n8n-mcp-skills-v1.0.0.zip              (168 KB) ★ RECOMMENDED
├── n8n-expression-syntax-v1.0.0.zip       (11 KB)
├── n8n-mcp-tools-expert-v1.0.0.zip        (15 KB)
├── n8n-workflow-patterns-v1.0.0.zip       (35 KB)
├── n8n-validation-expert-v1.0.0.zip       (18 KB)
├── n8n-node-configuration-v1.0.0.zip      (17 KB)
└── README.md                               (this file)
```

---

## 📋 What's Included in Each Package

### Individual Skill Packages (Claude.ai)

Each zip contains:
```
SKILL.md              # Main skill instructions with YAML frontmatter
[Reference files]     # Additional documentation and guides
README.md             # Skill metadata and statistics
```

### Bundle Package (Claude Code)

```
.claude-plugin/
  ├── plugin.json      # Claude Code plugin metadata
  └── marketplace.json # Marketplace listing metadata
README.md              # Project overview and documentation
LICENSE                # MIT License
skills/                # All 7 skills in subfolders
  ├── n8n-expression-syntax/
  ├── n8n-mcp-tools-expert/
  ├── n8n-workflow-patterns/
  ├── n8n-validation-expert/
  ├── n8n-node-configuration/
  ├── n8n-code-javascript/
  └── n8n-code-python/
```

## ✅ Verification

After installation, test skills by asking:

```
"How do I write n8n expressions?"
→ Should activate: n8n Expression Syntax

"Find me a Slack node"
→ Should activate: n8n MCP Tools Expert

"Build a webhook workflow"
→ Should activate: n8n Workflow Patterns

"How do I access webhook data in a Code node?"
→ Should activate: n8n Code JavaScript

"Can I use pandas in Python Code node?"
→ Should activate: n8n Code Python
```

## 🔧 Requirements

- **n8n-mcp MCP server** installed and configured ([Installation Guide](https://github.com/czlonkowski/n8n-mcp))
- **Claude Pro, Max, Team, or Enterprise** plan (for Claude.ai skills)
- **.mcp.json** configured with n8n-mcp server

## 📖 Documentation

For detailed installation instructions, see:
- Main README: `../README.md`
- Installation Guide: `../docs/INSTALLATION.md`
- Usage Guide: `../docs/USAGE.md`

## 🐛 Troubleshooting

**Claude.ai Error: "Zip must contain exactly one SKILL.md file"**
- Use the individual skill zips, not the bundle
- Each skill must be uploaded separately

**Claude Code: Skills not activating**
- Verify skills are in `~/.claude/skills/` directory
- Check that n8n-mcp MCP server is running
- Reload Claude Code

**Skills not triggering**
- Skills activate based on keywords in your queries
- Try more specific questions matching skill descriptions
- Check that SKILL.md files have correct frontmatter

## 📝 License

MIT License - see [LICENSE](../LICENSE).

### Notice status

Read-only inspection of the six tracked ZIPs found the full root MIT license in
the complete bundle, but no LICENSE/NOTICE/COPYING-named file in the five
individual ZIPs. This is a packaging observation, not a claim of comprehensive
third-party clearance. The root license names Copyright (c) 2025 Romuald
Członkowski; ensure its copyright and permission notice accompanies copies or
substantial portions, including standalone skill redistribution. Preserve
upstream/human/community credits and applicable nested notices; external tools
and independently sourced templates are not relicensed by this README. Existing
ZIP bytes are unchanged.

## 🙏 Credits

Conceived by Romuald Członkowski - https://www.aiadvisors.pl/en

Part of the [n8n-mcp project](https://github.com/czlonkowski/n8n-mcp).
