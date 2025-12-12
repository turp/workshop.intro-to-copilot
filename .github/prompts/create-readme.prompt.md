---
agent: 'agent'
description: 'Create a README.md file for the project'
---

## Role

You're a senior expert software engineer with extensive experience in open source projects. You excel at creating README files that are appealing, informative, technically accurate, and easy to scan. You understand that a great README gets developers from zero to productive in under 5 minutes.

## Analysis Steps

Before writing, thoroughly analyze:
1. **Project structure**: Examine the file organization, entry points, and configuration files
2. **Dependencies**: Review package.json, requirements.txt, or equivalent for tech stack
3. **Existing documentation**: Check for inline comments, JSDoc, docstrings, or docs/ folder
4. **Scripts and commands**: Identify build, test, run, and deployment commands
5. **Code patterns**: Understand the architecture, frameworks, and design patterns used
6. **Target audience**: Determine if this is for beginners, experienced developers, or both

## Content Structure

Create a README with these sections in order of priority:

### 1. Header (Required)
- Project name with logo/icon if available
- One-line value proposition (what problem does this solve?)
- Relevant badges (build status, coverage, version, license)
- Brief description (2-3 sentences maximum)

### 2. Quick Start (Critical)
**Goal**: Get users running the project in < 5 minutes
- Prerequisites (Node.js 18+, Python 3.11+, etc.)
- Installation command(s)
- Minimal working example
- Expected output or success indicators

### 3. Features (Optional but recommended)
- Bullet list of key capabilities
- Focus on user benefits, not implementation details
- Use concrete examples where possible

### 4. Installation (Detailed)
- Step-by-step instructions for different environments
- Common installation issues and solutions
- Verification steps

### 5. Usage (Required)
- Common use cases with code examples
- CLI commands or API usage patterns
- Configuration options explained
- Real-world examples that users can copy-paste

### 6. API Reference (If applicable)
- Public interfaces, functions, or endpoints
- Parameters, return values, and types
- Brief examples for each

### 7. Development (Optional)
- How to set up for local development
- How to run tests
- How to build/compile
- Project structure explanation

### 8. Troubleshooting (Recommended)
- Common errors and their solutions
- Debug mode or logging instructions
- Where to get help

## Writing Style Guidelines

1. **Be concise**: Every sentence must add value. Remove fluff.
2. **Be specific**: Use exact versions, precise commands, and concrete examples.
3. **Be accurate**: Test all commands and code snippets before including them.
4. **Front-load value**: Most important information first. Assume readers skim.
5. **Use active voice**: "Run `npm install`" not "The installation can be done by running..."
6. **Show, don't tell**: Code examples > descriptions. Screenshots > explanations.
7. **Assume smart, busy readers**: Skip obvious explanations. Respect their time.

## Quality Checklist

Before finalizing, verify:
- [ ] All code examples are syntactically correct
- [ ] All commands have been validated against the project
- [ ] Installation steps work from scratch (no assumed setup)
- [ ] Links are valid and point to correct locations
- [ ] Technical terms are accurate for the framework/language used
- [ ] Version numbers match actual dependencies
- [ ] Prerequisites are explicitly stated, not assumed

## Formatting Rules

1. Use GFM (GitHub Flavored Markdown) exclusively
2. Use GitHub admonitions for warnings/notes: https://github.com/orgs/community/discussions/16925
   ```markdown
   > [!NOTE]
   > Useful information that users should know
   
   > [!WARNING]
   > Critical information requiring immediate attention
   ```
3. Use fenced code blocks with language identifiers:
   ```bash
   npm install
   ```
4. Use tables for structured data (configuration options, API parameters)
5. Use minimal emojis (only if they add clarity, never decoration)
6. Use relative links for internal documentation files

## What NOT to Include

- ❌ LICENSE section (link to LICENSE file instead)
- ❌ CONTRIBUTING section (link to CONTRIBUTING.md instead)
- ❌ CHANGELOG section (link to CHANGELOG.md instead)
- ❌ "About the Author" or personal information
- ❌ Redundant information already in package.json
- ❌ Obvious statements ("This is a README file")
- ❌ Long philosophical explanations of design decisions (save for docs/architecture.md)

## Inspiration Examples

Reference these high-quality READMEs for structure and tone:
- https://raw.githubusercontent.com/Azure-Samples/serverless-chat-langchainjs/refs/heads/main/README.md
- https://raw.githubusercontent.com/Azure-Samples/serverless-recipes-javascript/refs/heads/main/README.md
- https://raw.githubusercontent.com/sinedied/run-on-output/refs/heads/main/README.md
- https://raw.githubusercontent.com/sinedied/smoke/refs/heads/main/README.md

## Final Task

Create a comprehensive README.md that a developer could use to understand, install, and start using this project in under 5 minutes. Prioritize accuracy and clarity over completeness. When in doubt, be concise.
 