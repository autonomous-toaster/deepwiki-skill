---
name: deepwiki
description: "Search 300+ indexed repositories for documentation and API references"
---

# DeepWiki Documentation Access

Search and retrieve comprehensive documentation from DeepWiki for popular GitHub repositories.

## Overview

DeepWiki (https://deepwiki.com/) provides AI-generated, navigable documentation for 300+ popular GitHub repositories. This skill enables you to:

1. **Search** for repositories in DeepWiki via API
2. **Fetch** documentation using mcporter MCP tools
3. **Retrieve** architecture guides, API references, and usage patterns

## Quick Start

### Search for a Repository

```bash
# Search for a specific repository
SEARCH_TERM="fastapi"
RESULTS=$(bash -c "curl -s 'https://api.devin.ai/ada/list_public_indexes?search_repo=$SEARCH_TERM'")
echo "$RESULTS" | jq '.indices[0]'
```

### Retrieve Documentation

```bash
# List available DeepWiki MCP tools
npx mcporter list https://mcp.deepwiki.com/mcp

# Fetch documentation for a repository
npx mcporter call https://mcp.deepwiki.com/mcp <tool-name> --args '{"org":"tiangolo","repo":"fastapi"}'
```

## How It Works

### 1. Repository Search

Search the DeepWiki index for repositories using the public API:

```bash
# API endpoint
https://api.devin.ai/ada/list_public_indexes?search_repo=SEARCH_TERM

# Returns JSON with indexed repositories
```

**API Response format:**
```json
{
  "indices": [
    {
      "id": "v1.9.9.5/PUBLIC/{org}/{repo}/{hash}",
      "repo_name": "{org}/{repo}",
      "last_modified": "2025-07-24T12:38:46.745692+00:00",
      "description": "...",
      "stargazers_count": 70369,
      "language": "JavaScript",
      "topics": ["tag1", "tag2"]
    }
  ],
  "needs_reindex": [],
  "pending_repos": []
}
```

**Selection strategy:**
- Sort by: highest stars → most recent update → exact name match
- Use best match for documentation retrieval

**Handling no results:**
If repo not found in DeepWiki, inform user: "Repository not indexed in DeepWiki. Available similar repos: [list alternatives if any]"

### 2. Discover MCP Tools

Use mcporter to list available tools from the DeepWiki MCP server:

```bash
# List all available DeepWiki MCP tools
npx mcporter list https://mcp.deepwiki.com/mcp
```

This shows available tools for fetching different types of documentation (API references, guides, examples, etc.).

### 3. Fetch Documentation

Use mcporter to call DeepWiki MCP tools with repository information:

```bash
# Call MCP tool to fetch documentation
npx mcporter call https://mcp.deepwiki.com/mcp <tool-name> --args '{"org":"<org>","repo":"<repo>"}'
```

## Common Use Cases

### Look Up API Documentation

```bash
# Search for a library
curl -s 'https://api.devin.ai/ada/list_public_indexes?search_repo=axios' | jq '.indices[0]'

# Fetch documentation with mcporter
npx mcporter call https://mcp.deepwiki.com/mcp get_documentation --args '{"org":"axios","repo":"axios"}'
```

### Find Architecture Patterns

```bash
# Search for a framework
curl -s 'https://api.devin.ai/ada/list_public_indexes?search_repo=django' | jq '.indices[0]'

# Retrieve architectural guides
npx mcporter call https://mcp.deepwiki.com/mcp get_architecture --args '{"org":"django","repo":"django"}'
```

### Explore Usage Examples

```bash
# Search for a tool
curl -s 'https://api.devin.ai/ada/list_public_indexes?search_repo=kubernetes' | jq '.indices[0]'

# Get common patterns and examples
npx mcporter call https://mcp.deepwiki.com/mcp get_examples --args '{"org":"kubernetes","repo":"kubernetes"}'
```

## Query Optimization

**Exact match searches:**
Include the exact repository name for better results
```bash
search_repo=fastapi  # Good: exact match
search_repo=fast    # Less optimal: partial match
```

**Multi-word searches:**
Use underscores or dashes (as they appear on GitHub)
```bash
search_repo=machine-learning-models
search_repo=computer_vision
```

**Sorting results:**
Results are automatically sorted by relevance; use jq to filter:
```bash
# Get top 5 results by stars
echo "$RESULTS" | jq '.indices | sort_by(-(.stargazers_count // 0)) | .[0:5]'

# Get most recently updated
echo "$RESULTS" | jq '.indices | sort_by(-(.last_modified // "")) | .[0]'
```

#### Limitations & Notes

- **Coverage:** Only 300 most popular repositories indexed
- **Quality:** DeepWiki docs are AI-generated; quality varies by repo
- **Selection:** Multi-repo matches use automatic selection (stars → recency → name)
- **Errors:** Non-indexed repos inform user; no fallback to other sources

## Troubleshooting

**Q: Repository not found in DeepWiki**
A: Only 300 most popular repos are indexed. Check if repo is in top 300 by stars on GitHub.

**Q: Documentation seems incomplete**
A: DeepWiki docs are AI-generated. Check the original GitHub repo for complete documentation.

**Q: mcporter command fails**
A: Ensure mcporter is installed: `npm install -g mcporter`. Check that the MCP server URL is correct.

**Q: What's the difference between DeepWiki and other documentation sources?**
A: DeepWiki specializes in comprehensive architecture and design documentation for popular repos. Use Context7 for quick code snippets and implementation details.

## Quick Reference

### Search a Repository
```bash
bash -c "curl -s 'https://api.devin.ai/ada/list_public_indexes?search_repo=fastapi'" | jq '.indices[0]'
```

### List DeepWiki MCP Tools
```bash
npx mcporter list https://mcp.deepwiki.com/mcp
```

### Call DeepWiki MCP Tool
```bash
npx mcporter call https://mcp.deepwiki.com/mcp <tool-name> --args '{"org":"<org>","repo":"<repo>"}'
```

### Extract Repository Info
```bash
# Extract org and repo name from search results
RESULT=$(bash -c "curl -s 'https://api.devin.ai/ada/list_public_indexes?search_repo=fastapi'")
REPO_NAME=$(echo "$RESULT" | jq -r '.indices[0].repo_name')
ORG=$(echo "$REPO_NAME" | cut -d'/' -f1)
REPO=$(echo "$REPO_NAME" | cut -d'/' -f2)
echo "org=$ORG, repo=$REPO"
```
