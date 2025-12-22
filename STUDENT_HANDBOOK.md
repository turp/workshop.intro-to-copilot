# GitHub Copilot Workshop - Student Handbook

**Beyond the Basics: Practical Exercises & Key Concepts**

> **Get Your License**: https://developer.aa.com/access/github-copilot

---

## Table of Contents
1. [Quick Start Guide](#quick-start-guide)
2. [Understanding Copilot Agents](#understanding-copilot-agents)
3. [Workshop Exercises](#workshop-exercises)
4. [Key Concepts & Techniques](#key-concepts--techniques)
5. [Best Practices Checklist](#best-practices-checklist)
6. [Quick Reference](#quick-reference)

---

## Quick Start Guide

### Prerequisites
- GitHub Copilot license (get yours at the link above)
- VS Code with GitHub Copilot extension installed
- Basic familiarity with your programming language

### Recommended Model
**Claude Sonnet 4.x** - Best for complex reasoning, code understanding, and multi-file refactoring

### Four Core Agents

| Agent | Purpose | When to Use |
|-------|---------|-------------|
| **Ask** | Answer questions, explain concepts | Need info without code changes |
| **Plan** | Create implementation roadmap | Want to see plan before changes |
| **Agent** | Autonomous multi-file coding | Complex tasks spanning multiple files |
| **Edit** | Targeted file modifications | Focused changes to specific files |

**Key Insight:** Asking agents (Ask, Plan) inform. Editing agents (Agent, Edit) modify code directly.

---

## Understanding Copilot Agents

### Ask Agent - Your Knowledge Expert
**What it does:** Answers questions without modifying code

**Best for:**
- "How do I parse JSON in Python?"
- "What's the difference between async/await and promises?"
- "Explain this error message"
- Getting code snippets to copy manually

**Example:**
```
How do I implement JWT authentication in Express?
```

### Plan Agent - Your Strategic Planner
**What it does:** Creates detailed implementation plans

**Best for:**
- Breaking down complex features
- Understanding refactor scope
- Planning architecture changes
- Estimating multi-file impact

**Example:**
```
Plan how to add user authentication to this React app with:
- Login/signup pages
- Protected routes
- JWT token storage
- API integration
```

### Agent Mode - Your Autonomous Developer
**What it does:** Searches, reads, and edits multiple files autonomously

**Best for:**
- Complex refactoring across files
- Debugging that requires codebase understanding
- Multi-file implementations
- Running terminal commands
- Installing dependencies

**Example:**
```
Refactor this API to use async/await and add comprehensive error 
handling throughout the service layer. Update all affected files 
and run tests to verify.
```

### Edit Agent - Your Focused Editor
**What it does:** Makes targeted modifications to specific files

**Best for:**
- Quick refactors in current file
- Applying changes to selected code
- Iterative improvements
- Focused edits without broad analysis

**Example:**
```
Add input validation to this function with detailed error messages
```

---

## Workshop Exercises

### Exercise: Generate Professional README

**Objective:** Learn how agents use workspace context to create tailored documentation

**Task:**
1. Open Copilot Chat in Agent mode
2. Add `#codebase` context
3. Use this prompt:

```markdown
Create a comprehensive README.md for this project. Include:
- Project overview and purpose
- Installation instructions
- Usage examples with code snippets
- API documentation
- Contributing guidelines
- License information

Analyze the codebase to understand the tech stack and dependencies.
```

**What You'll Learn:**
- How agents analyze your project structure
- Context-aware documentation generation
- Tailoring output to your specific codebase

---

### Exercise: Project Assessment & Technical Debt Analysis

**Objective:** Use Copilot to perform comprehensive codebase analysis

**The Scenario:** You've just inherited a project, or you need to assess the current state of your application before planning the next sprint.

**Task:**
Use this prompt with Agent mode and `#codebase` context:

```markdown
Analyze this codebase and provide:
1. Architecture overview (patterns, structure)
2. Technology stack and dependencies
3. Code quality assessment (maintainability, test coverage)
4. Security concerns or vulnerabilities
5. Technical debt areas
6. Recommended improvements prioritized by impact
```

**What Copilot Will Identify:**
- Outdated dependencies with known vulnerabilities
- Code smells and anti-patterns
- Missing error handling or validation
- Architectural inconsistencies
- Areas with low test coverage
- Performance bottlenecks

**Real-World Value:** 
This analysis can inform sprint planning, help prioritize technical debt paydown, and provide ammunition for convincing stakeholders to allocate time for improvements.

**What You'll Learn:**
- Rapid codebase assessment techniques
- Identifying technical debt systematically
- Understanding architectural patterns in existing code

---

### Exercise: Automated Test Generation

**Objective:** Generate comprehensive test suites efficiently

**The Scenario:** Test coverage is crucial, but writing tests is often deprioritized. Copilot can generate comprehensive test suites that significantly reduce the time investment.

**Basic Prompt:**
```markdown
Generate unit tests for the `UserService` class. Include:
- Happy path scenarios
- Edge cases
- Error handling
- Mock external dependencies
Use [Jest/NUnit/JUnit - specify your framework]
```

**Advanced Prompt for Comprehensive Coverage:**
```markdown
Create comprehensive test suite for this module:
1. Unit tests with 80%+ coverage
2. Integration tests for API endpoints
3. Test data factories/fixtures
4. Mock setup for database and external APIs
5. Follow AAA pattern (Arrange, Act, Assert)
```

**Important Reminder:** While Copilot generates excellent test scaffolding and common test cases, you must still review the tests to ensure they validate your specific business logic correctly. Copilot understands common patterns but doesn't understand your unique business requirements.

**What You'll Learn:**
- Efficient test suite generation
- Test patterns and best practices
- Mock setup strategies
- Coverage optimization techniques

---

### Exercise: Meta-Prompting (Let Copilot Write Your Prompts)

**Objective:** Learn advanced prompt engineering by having AI help write better prompts

**The Concept:** Copilot knows what context it needs to generate better code. By asking it to write prompts, you learn to provide better instructions and ensure you don't miss critical details.

**Task:**
Use this prompt:

```markdown
Create a prompt that I can use to create unit tests for my project. Goal is 80%+ coverage. Follow AAA pattern. Use the builder pattern to create test data.
```

**Why This Works:**
- Copilot knows its own capabilities and limitations
- In agent mode, it will look at existing tests for guidance

**Real-World Applications:**
- Creating project templates for your team
- Establishing coding standards that Copilot can follow
- Documenting complex requirements before implementation
- Training junior developers on how to communicate technical requirements

**What You'll Learn:**
- How to structure effective prompts
- What context AI needs for better results
- Codifying institutional knowledge

---

### Exercise: Reusable Assets: Create a Reusable Prompt

**Objective:** Build reusable prompt templates that make it easy for team to share 

**Task:**
1. Create a file: `.github/prompts/create-unit-tests.prompt.md`
2. Ask Copilot to generate the prompt:

```markdown
Create a reusable prompt to generate unit tests for this project. 
If a file name is not provided, ask for it.
```

3. Test your prompt:
```
/create-unit-tests UserService.cs
```

**Review Checklist:**
- [ ] Tests follow your team's patterns?
- [ ] Coverage includes happy path, edge cases, errors?
- [ ] Mocks used appropriately?
- [ ] Test names clearly describe what's tested?
- [ ] Assertions are specific and meaningful?

**Next Steps:**
1. Document usage in contribution guide
2. Commit to `.github/prompts/`
3. Gather team feedback
4. Iterate based on test needs

---

### Exercise: Reusable Assets: Create Custom Instructions - Coding Standards

**Objective:** Define instruction files that guide Copilot's code generation

**Task:**
1. Create: `.github/instructions/code-standards.instructions.md`
2. Use this prompt:

```markdown
Create comprehensive code standards for this project. Include:
- File and directory structure conventions
- Naming conventions (variables, functions, classes, files)
- Code formatting and style guidelines
- Best practices and design patterns
- Error handling standards
- Testing requirements
- Documentation and comment standards
- Common anti-patterns to avoid

Use clear, actionable language with code examples.
```

**Review Checklist:**
- [ ] All major code elements covered?
- [ ] Error handling defined?
- [ ] Testing expectations clear?
- [ ] Aligns with official style guides?
- [ ] Code examples use correct syntax?
- [ ] Rules are specific and actionable?
- [ ] Language is imperative ("Use X", not "Consider X")?

**Extra Credit:**
- Use different model to review for conflicting language
- Ask Copilot to create linting rules to enforce standards

---

### Exercise: Reusable Assets: Create a Custom Agent

**Objective:** Create specialized AI assistants for specific workflows

**Task:**
1. Choose a focused task (writing tests, documentation, linting)
2. Create: `.github/agents/[name].agent.md`
3. Use this template prompt:

```markdown
Create a Python development agent file. The agent should:
- Write Python code following PEP 8 standards
- Use type hints and docstrings
- Write pytest tests
- Run commands: pytest, black, pylint
- Never modify .env files or commit secrets
```

**Agent File Structure:**
```markdown
---
name: your-agent-name
description: One-sentence description
---

You are an expert [role] for this project.

## Your role
- Specialize in [specific domain]
- Understand [technology/patterns]
- Task: [clear objective]

## Project knowledge
- Tech Stack: [technologies with versions]
- Key directories and their purposes

## Commands you can use
- Build: `npm run build`
- Test: `npm test`
- Lint: `npm run lint --fix`

## Code style
[Provide real examples of good vs bad code]

## Boundaries
- ✅ Always do: [safe operations]
- ⚠️ Ask first: [operations requiring confirmation]
- 🚫 Never do: [forbidden operations]
```

**Tips for Great Agents:**
1. **Put commands early** - Include executable commands with flags
2. **Code examples over explanations** - Show don't tell
3. **Set clear boundaries** - What to never touch (secrets, configs)
4. **Be specific about stack** - Include versions and dependencies
5. **Cover six areas** - Commands, testing, structure, style, git, boundaries

---

### Final Exercise: Security Vulnerability Automation

**Objective:** Utilize **meta-prompting** and **custom agents** to help you automate a common task encountered by many teams.

**The Scenario:**
Peridically, someone on your team checks `Nuclei` to see if there are any new security vulerabilities that need to be resolved. Let's use Copilot to automate this repetive task and free you up to other interesting things. 

**Your Task:**
Ask Copilot to:
* create a github action to periodically check `Nuclei` for new vulerabilities
* create an issue for each vulenability found
* assign each issue to your `Github Copilot Agent` for it to work on

This allows the `agent` to create a branch and a pull request and work on resolving the vulerability. You guide it via comments in the PR. Once you are satisfied with the solution the agent has produced, you do a final review of the PR and merge it into the main branch.

Execute the following prompt: 

```markdown
create a prompt that will create a GitHub action that will check nucleus for Critical/High vulnerabilities on a daily basis and will create GitHub issues for each vulnerabilities and assign it to an agent to resolve.  
```

**What to Expect:**
Different models provide different approaches:
- **Gemini**: Focuses on official Nuclei Action, GitHub CLI
- **Claude**: Emphasizes comprehensive lifecycle management, adaptable approach

**Learning Objectives:**
- Practice meta-prompting for better prompt engineering
- Break down complex automation requirements
- Evaluate different AI approaches
- Create reusable security automation

**Next Steps:**
1. Try with different models (Claude, GPT-4, Gemini)
2. Use generated prompt to create the actual GitHub Action
3. Review for security best practices (secrets, permissions)
4. Adapt pattern for other tools (Snyk, Trivy, Dependabot)

---

## Key Concepts & Techniques

### The Mindset Shift

**Before:** "I use AI to write code faster"
**After:** "I use AI as a force multiplier while I focus on architecture and strategy"

| Your Focus | AI's Focus |
|------------|------------|
| Architecture decisions | Implementation details |
| Business logic design | Boilerplate code |
| Code review and quality | Test generation |
| Security requirements | Common vulnerability patterns |
| Performance optimization | Initial refactoring drafts |

### Five Time-Saving Techniques

#### 1. Context-Rich Prompting
❌ Weak: "Write a FastAPI endpoint for creating users"
✅ Strong: "You're implementing the User Management module. The system uses JWT auth and PostgreSQL via SQLAlchemy. Create a FastAPI endpoint for creating users with validation..."

**Pro Tip:** Share design docs, architecture files before requesting code

#### 2. Dual-Perspective Review
- Generate code with one model
- Review with fresh session or different model
- Catches logical errors and edge cases early
- Reduces human review overhead

#### 3. Automate Tests & Validation
```markdown
Analyze test coverage for UserService.js and generate tests for:
- Uncovered edge cases
- Error handling paths
- Input validation scenarios
- Mock setup for external dependencies
```

#### 4. AI-Driven Refactoring
```markdown
Refactor this code to:
1. Extract business logic into service layer
2. Add TypeScript types
3. Reduce database queries
4. Implement Repository pattern

Maintain current functionality - do not change behavior.
Add tests to verify equivalence.
```

#### 5. Parallel AI Sessions
- Open multiple chat sessions for different tasks
- Continue working while AI processes requests
- Review and integrate results as they complete
- Stay in flow state on primary task

### Choosing the Right Interaction

| Task | Use | Why |
|------|-----|-----|
| Quick syntax question | Ask agent | Fast, focused |
| Refactoring multiple files | Agent mode | Autonomous, reads across files |
| Debugging complex issues | Agent mode | Requires codebase search |
| Explaining concept | Ask agent | Straightforward answer |
| Planning complex feature | Plan agent | Creates roadmap |
| Focused edit in current file | Inline chat (Ctrl+I) | Keeps you in flow |
| Targeted file changes | Edit agent | Focused modifications |

### Context References

Add specific context to prompts:
- `#codebase` - Full workspace context
- `#file` - Specific file
- `#selection` - Selected code
- `#terminalSelection` - Terminal output

---

## Best Practices Checklist

### Before Using Copilot
- [ ] Choose appropriate agent for task (Ask, Plan, Agent, Edit)
- [ ] Add relevant context (`#codebase`, `#file`)
- [ ] Open related files in editor
- [ ] Have clear objective in mind

### During Code Generation
- [ ] Provide clear, detailed prompts
- [ ] Include tech stack, versions, requirements
- [ ] Specify error handling and edge cases
- [ ] Mention testing framework and patterns
- [ ] Request security considerations upfront

### After Code Generation - The 5-Minute Review

#### ✅ 1. Does It Work?
- [ ] Compiles/runs without errors
- [ ] Tests pass (existing and new)
- [ ] No runtime exceptions
- [ ] Handles edge cases

#### ✅ 2. Is It Secure?
- [ ] No hardcoded secrets
- [ ] Input validation present
- [ ] SQL/XSS injection protection
- [ ] Authentication checks in place
- [ ] No sensitive data in logs

#### ✅ 3. Follows Standards?
- [ ] Matches team naming conventions
- [ ] Follows file/folder structure
- [ ] Uses approved libraries
- [ ] Consistent with existing style
- [ ] Proper error handling

#### ✅ 4. Tests Are Meaningful?
- [ ] Validates business logic, not just syntax
- [ ] Edge cases covered
- [ ] Not over-reliant on mocks
- [ ] Test names are descriptive
- [ ] Assertions are specific

#### ✅ 5. Ready to Commit?
- [ ] Reviewed diff carefully (`git diff --staged`)
- [ ] No unintended file changes
- [ ] Commit message describes change
- [ ] Can explain change in one sentence
- [ ] 50-100 lines changed (small batches!)

### Security Considerations

**Never share in prompts:**
- ❌ API keys or secrets
- ❌ Passwords
- ❌ Connection strings with credentials
- ❌ Personally identifiable information (PII)
- ❌ Proprietary business logic

**Always review:**
- Authentication/authorization logic
- Input validation and sanitization
- Cryptographic operations
- Database queries (injection risks)
- File system operations

---

## Quick Reference

### Keyboard Shortcuts (VS Code)
- `Ctrl+I` (Cmd+I on Mac) - Inline chat in editor
- `Ctrl+Shift+I` - Open Copilot chat view
- `Tab` - Accept inline suggestion
- `Alt+]` - Next suggestion
- `Alt+[` - Previous suggestion

### Common Prompt Patterns

**Code Review:**
```markdown
Review this code for:
1. Bugs and logic errors
2. Performance issues
3. Security vulnerabilities
4. Best practice violations
5. Readability and maintainability

Suggest specific improvements with examples.
```

**Debugging:**
```markdown
Analyze this error and suggest fixes:

[Error message and stack trace]

Context:
- [Framework/library info]
- [When error occurs]
- [Expected vs actual behavior]

Provide:
1. Root cause analysis
2. Specific code fix
3. Prevention strategies
4. Logging improvements
```

**Refactoring:**
```markdown
Refactor this code to:
1. [Specific improvement]
2. [Another improvement]
3. [Performance goal]

Maintain current functionality exactly.
Add tests to verify equivalence.
```

**Test Generation:**
```markdown
Generate comprehensive test suite for [module]:
1. Unit tests with 80%+ coverage
2. Happy path and edge cases
3. Error handling tests
4. Mock external dependencies
5. Follow AAA pattern (Arrange, Act, Assert)

Use [Jest/NUnit/JUnit]
```

### Smart Actions in VS Code

Available AI-enhanced shortcuts:
- **Commit Messages** - Right-click staged changes → Generate
- **Fix Errors** - Click lightbulb next to errors
- **Rename Symbol** - F2 for AI-powered naming
- **Semantic Search** - Natural language code search
- **PR Descriptions** - Generate from changes

### Model Selection Guide

- **Claude Sonnet 4.x** - Complex reasoning, refactoring, architecture (Recommended)
- **Claude Haiku 4.x** - Fast responses for simple tasks
- **GPT-4.1** - Creative solutions, alternative perspectives
- **O1 Models** - Advanced reasoning for algorithms (use sparingly)

**Pro Tip:** Try different models if first result doesn't meet needs

### Building Reusable Assets

**File Structure:**
```
.github/
├── prompts/              # Reusable prompt templates
│   ├── create-readme.prompt.md
│   ├── generate-tests.prompt.md
│   └── code-review.prompt.md
├── instructions/         # Coding standards & guidelines
│   ├── testing.instructions.md
│   ├── api-design.instructions.md
│   └── security.instructions.md
└── agents/              # Custom specialized agents
    ├── docs-agent.md
    ├── test-agent.md
    └── security-agent.md
```

**Benefits:**
- Enforce standards automatically
- Instant onboarding for new developers
- Reduce context switching
- Improve code review quality
- Create institutional knowledge

### Workflow Pattern

```
1. AI generates code (50-100 lines)
   ↓
2. YOU review (does it work, is it secure, does it fit?)
   ↓
3. YOU test (run tests, verify behavior)
   ↓
4. YOU commit (one logical unit, every 10-15 min)
   ↓
5. REPEAT
```

**Critical Rule:** Never let AI-generated code accumulate without commits. Commit every 10-15 minutes during active development.

---

## Additional Resources

### Community Resources
- **Awesome Copilot**: https://github.com/github/awesome-copilot
- Industry-specific prompts and instructions
- Framework-specific configurations
- Community best practices

### Getting Help
- GitHub Copilot Documentation: https://docs.github.com/copilot
- VS Code Copilot Extension: In VS Code, search "GitHub Copilot"
- License Access: https://developer.aa.com/access/github-copilot

### Remember
- Start small, build incrementally
- Review all generated code
- Commit frequently (small batches)
- Build reusable assets for your team
- Experiment with different models
- Always prioritize security

---

**Happy Coding! 🚀**

*This handbook is a companion to the full workshop guide. For comprehensive information, see README.md*
