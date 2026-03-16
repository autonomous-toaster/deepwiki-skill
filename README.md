# DeepWiki Skill Package

Search and retrieve comprehensive documentation from 300+ indexed GitHub repositories including architecture guides, API references, and usage patterns.

## Installation

### Global
```bash
pi install npm:@autonomous-toaster/deepwiki-skill
```

### Project
```bash
pi install -l npm:@autonomous-toaster/deepwiki-skill
```

### Trial (Temporary)
```bash
pi -e npm:@autonomous-toaster/deepwiki-skill
```

## Requirements

- **mcporter** - Install with `npm install -g mcporter`
- pi coding agent

## Features

- Search DeepWiki's index of 300+ popular repositories
- Access AI-generated comprehensive documentation
- Retrieve architecture guides and design patterns
- Find API references and implementation examples
- Automatic repository selection by popularity and recency

## Usage

Once installed, use the skill with prompts like:

- "Show me the architecture of FastAPI"
- "Find documentation for Kubernetes patterns"
- "What are the best practices in the Next.js framework?"
- "Search DeepWiki for React architecture guide"
- "How does Django handle database migrations?"

## Setup

### Install mcporter

```bash
npm install -g mcporter
```

### Verify Setup

```bash
# Test mcporter installation
npx mcporter list https://mcp.deepwiki.com/mcp

# Test API connectivity
curl -s 'https://api.devin.ai/ada/list_public_indexes?search_repo=fastapi' | jq '.indices[0]'
```

## Supported Repositories

DeepWiki indexes 300+ popular repositories across:

- **Web Frameworks**: FastAPI, Django, Next.js, Express, Rails
- **Data Science**: TensorFlow, PyTorch, Pandas, Scikit-learn
- **Containers**: Docker, Kubernetes
- **Programming Languages**: Python, JavaScript, Rust, Go
- And many more...

## How It Works

1. **Search** - Find repositories in DeepWiki index
2. **Extract** - Get org/repo names from search results
3. **Retrieve** - Call MCP tools to fetch documentation
4. **Apply** - Use architecture guides and patterns in your work

## Troubleshooting

**Repository not indexed?**
Only the 300 most popular repositories by GitHub stars are indexed. Check if your target repo is in the top 300.

**Documentation incomplete?**
DeepWiki generates AI documentation. Cross-reference with the official GitHub repository for complete information.

**mcporter not found?**
Install with `npm install -g mcporter`

## More Information

See the included `SKILL.md` file for detailed API documentation and advanced usage patterns.
