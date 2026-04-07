# MCP Server Setup — Design System Skill

> **TL;DR:** Optional MCP server connections that enhance agent capabilities when using this skill. shadCN MCP enables component browsing, searching, and installation via natural language. GitHub MCP enables repository operations (commits, PRs, branch management). Both are optional — the skill works without them — but they are recommended for workflows like Storybook generation and theme creation.

## Purpose

Setup instructions for MCP (Model Context Protocol) servers that complement this design system skill. These connections are optional but recommended for certain workflows.

**Use for:** Configuring shadCN MCP and GitHub MCP in your IDE/agent environment.

**Do not use for:** Understanding MCP protocol itself, building custom MCP servers.

---

## When Are MCP Connections Needed?

| Workflow | shadCN MCP | GitHub MCP |
|----------|-----------|------------|
| Generate a Storybook | Recommended (verify installed components) | Optional (commit story files) |
| Generate a New Theme | Not needed | Optional (commit theme files) |
| Add components to project | Recommended (browse and install) | Not needed |
| General design system reference | Not needed | Not needed |

If an MCP connection is unavailable, the agent will notify you and proceed using local file system operations and skill content as the source of truth.

---

## Configure shadCN MCP Server

The shadCN MCP server allows agents to browse, search, and install components from shadcn-compatible registries.

Source: `https://ui.shadcn.com/docs/mcp`

### Prerequisites

- Node.js and npm installed (npx is included with npm)
- A valid `components.json` in the project root (run `npx shadcn@latest init` if missing)

### Configuration by IDE

**Kiro** — add to `.kiro/settings/mcp.json` (workspace) or `~/.kiro/settings/mcp.json` (user):

```json
{
  "mcpServers": {
    "shadcn": {
      "command": "npx",
      "args": ["shadcn@latest", "mcp"]
    }
  }
}
```

**Cursor** — add to `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "shadcn": {
      "command": "npx",
      "args": ["shadcn@latest", "mcp"]
    }
  }
}
```

**VS Code (GitHub Copilot)** — add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "shadcn": {
      "command": "npx",
      "args": ["shadcn@latest", "mcp"]
    }
  }
}
```

**Claude Code** — add to `.mcp.json`:

```json
{
  "mcpServers": {
    "shadcn": {
      "command": "npx",
      "args": ["shadcn@latest", "mcp"]
    }
  }
}
```

After adding the configuration, restart your IDE or reconnect the MCP server.

---

## Configure GitHub MCP Server

The GitHub MCP server allows agents to interact with GitHub repositories — commits, branches, pull requests, and file operations.

Source: `https://www.npmjs.com/package/@modelcontextprotocol/server-github`

### Prerequisites

- Node.js and npm installed
- A GitHub Personal Access Token (PAT) with appropriate repository permissions

### Configuration by IDE

**Kiro** — add to `.kiro/settings/mcp.json` (workspace) or `~/.kiro/settings/mcp.json` (user):

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "<YOUR_PERSONAL_ACCESS_TOKEN>"
      }
    }
  }
}
```

**Cursor** — add to `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "<YOUR_PERSONAL_ACCESS_TOKEN>"
      }
    }
  }
}
```

**VS Code (GitHub Copilot)** — add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "<YOUR_PERSONAL_ACCESS_TOKEN>"
      }
    }
  }
}
```

**Claude Code** — add to `.mcp.json`:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "<YOUR_PERSONAL_ACCESS_TOKEN>"
      }
    }
  }
}
```

### Create a Personal Access Token

1. Go to GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens
2. Click "Generate new token"
3. Set repository access to the repositories you need
4. Grant permissions: Contents (Read and write), Metadata (Read-only), Pull requests (Read and write)
5. Generate and copy the token immediately
6. Replace `<YOUR_PERSONAL_ACCESS_TOKEN>` in the config above

### Security

- NEVER include actual tokens, secrets, or passwords in any file or commit
- Always use the placeholder `<YOUR_PERSONAL_ACCESS_TOKEN>` in documentation
- Scope tokens to minimum required permissions
- Tokens expire — regenerate if you see 401/403 errors

---

## References

| Source | URL | Used For |
|--------|-----|----------|
| shadCN MCP | `https://ui.shadcn.com/docs/mcp` | shadCN MCP server setup |
| GitHub MCP | `https://www.npmjs.com/package/@modelcontextprotocol/server-github` | GitHub MCP server setup |
