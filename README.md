# GitHub Copilot: Beyond the Basics - A Comprehensive Guide

**An Introduction to Advanced Copilot Features for Developers**

> **Get Your License**: https://developer.aa.com/access/github-copilot

---

## Table of Contents
1. [Getting Started: IDE Integration](#1-getting-started-ide-integration)
2. [Practical Examples & Use Cases](#2-practical-examples--use-cases)
3. [Building Reusable Assets](#3-building-reusable-assets)
4. [Best Practices & Tips](#4-best-practices--tips)
5. [Next Steps & Resources](#5-next-steps--resources)

---

## 1. Getting Started: IDE Integration

GitHub Copilot has evolved far beyond simple code completion. To harness its full potential, understanding how to integrate it with your development environment and leverage its different modes is essential.

### Supported IDEs

GitHub Copilot integrates seamlessly with several popular development environments, each offering unique advantages:

**Visual Studio Code** - The most feature-rich integration, offering the complete Copilot experience with full access to all chat modes, inline suggestions, and agent capabilities. If you're looking for the most comprehensive Copilot experience, VS Code is your best choice.

**Visual Studio** - Provides full support tailored for .NET developers, with deep integration into the Visual Studio ecosystem. Perfect for C# and .NET development workflows.

**IntelliJ IDEA** - Offers a Java and Kotlin-optimized experience, with intelligent context awareness for JVM-based projects.

**Other IDEs** - Copilot also supports Neovim and the broader JetBrains suite, ensuring developers can use their preferred tools.

### Understanding Chat Modes: Ask vs Agent

One of the most important concepts to grasp when working with GitHub Copilot is the difference between its two primary chat modes: **Ask Mode** and **Agent Mode**. Choosing the right mode for your task can dramatically improve your productivity.

#### Ask Mode: Quick Questions and Fast Answers

Ask Mode is designed for single-turn conversations where you need a quick, focused answer. Think of it as asking a knowledgeable colleague a specific question.

**Best for:**
- Syntax questions ("How do I parse JSON in Python?")
- Quick explanations of code concepts
- Code snippets for specific tasks
- API usage examples

**When to use Ask Mode:** Use this mode when you know exactly what you need and can articulate it in a single question. The response will be immediate and focused on your specific query without extensive analysis of your codebase.

#### Agent Mode: Autonomous Problem Solving

Agent Mode represents a more powerful, autonomous approach to coding assistance. In this mode, Copilot can perform multi-step problem solving, read files in your workspace, search your codebase for context, and even make edits across multiple files.

**Best for:**
- Refactoring complex code structures
- Debugging issues that require understanding of your codebase
- Complex implementations that span multiple files
- Code analysis and architecture reviews

**Example scenario:** Instead of asking "How do I add error handling?", you can request: "Refactor this API to use async/await and add comprehensive error handling throughout the service layer."

**The key difference:** Agent Mode understands your project context and can take autonomous actions, while Ask Mode provides targeted answers to specific questions.

### Model Selection: Choosing the Right AI

GitHub Copilot offers multiple AI models, each with different strengths:

**Default Model: Claude Sonnet 4.x or Claude Haiku 4.x**
- Excels at code understanding and complex reasoning
- Better at architectural decisions and design patterns
- Strong performance with legacy code analysis

**Alternative: GPT-4.1**
- Sometimes better at creative solutions
- May excel at specific use cases or languages

**When to switch models:** Different models excel at different tasks. If you're not getting the results you want with one model, try switching to see if the alternative better suits your specific use case. Some developers find that Claude performs better for refactoring and understanding existing code, while GPT-4o might offer more creative approaches to new implementations.

> **Note:** Model availability may vary based on your organization's settings. Check with your administrator if you need access to specific models.

---

## 2. Practical Examples & Use Cases

The true power of GitHub Copilot becomes evident when you see it in action. This section provides real-world examples that you can adapt to your daily development tasks.

### Example 1: Generate Professional README Files

One of the most common yet time-consuming tasks in software development is creating comprehensive documentation. GitHub Copilot can analyze your entire codebase to generate professional README files that accurately reflect your project.

**The Scenario:** You've built a project, but it lacks proper documentation. New team members struggle to onboard, and even you sometimes forget how specific features work.

**Prompt Template:**
```
Create a comprehensive README.md for this project. Include:
- Project overview and purpose
- Installation instructions
- Usage examples with code snippets
- API documentation
- Contributing guidelines
- License information

Analyze the codebase to understand the tech stack and dependencies.
```

**How to use this:**
1. Open your project in VS Code
2. Switch to Agent mode in the Copilot chat
3. Paste the prompt above
4. Review the generated content and refine as needed

**Key Insight:** Because Agent mode can read your files, it will analyze your package.json, source files, and project structure to create documentation that actually matches your implementation. You're not getting generic templates – you're getting documentation tailored to your specific project.

### Example 2: Project Assessment & Technical Debt Analysis

Inheriting a legacy codebase or preparing for a major refactor? Copilot can provide a comprehensive analysis that would typically take hours of manual review.

**The Scenario:** You've just inherited a project, or you need to assess the current state of your application before planning the next sprint.

**Prompt Template:**
```
Analyze this codebase and provide:
1. Architecture overview (patterns, structure)
2. Technology stack and dependencies
3. Code quality assessment (maintainability, test coverage)
4. Security concerns or vulnerabilities
5. Technical debt areas
6. Recommended improvements prioritized by impact
```

**What Copilot will identify:**
- Outdated dependencies with known vulnerabilities
- Code smells and anti-patterns
- Missing error handling or validation
- Architectural inconsistencies
- Areas with low test coverage
- Performance bottlenecks

**Real-world value:** This analysis can inform sprint planning, help prioritize technical debt paydown, and provide ammunition for convincing stakeholders to allocate time for improvements.

### Example 3: Automated Test Generation

Test coverage is crucial, but writing tests is often deprioritized. Copilot can generate comprehensive test suites that significantly reduce the time investment.

**Basic Prompt:**
```
Generate unit tests for the `UserService` class. Include:
- Happy path scenarios
- Edge cases
- Error handling
- Mock external dependencies
Use [Jest/NUnit/JUnit - specify your framework]
```

**Advanced Prompt for Comprehensive Coverage:**
```
Create comprehensive test suite for this module:
1. Unit tests with 80%+ coverage
2. Integration tests for API endpoints
3. Test data factories/fixtures
4. Mock setup for database and external APIs
5. Follow AAA pattern (Arrange, Act, Assert)
```

**Example Output:**
```javascript
describe('UserService', () => {
  let userService;
  let mockDatabase;
  
  beforeEach(() => {
    mockDatabase = {
      findUser: jest.fn(),
      createUser: jest.fn()
    };
    userService = new UserService(mockDatabase);
  });

  describe('createUser', () => {
    it('should create user with valid data', async () => {
      // Arrange
      const userData = { email: 'test@example.com', name: 'Test User' };
      mockDatabase.createUser.mockResolvedValue({ id: 1, ...userData });
      
      // Act
      const result = await userService.createUser(userData);
      
      // Assert
      expect(result.id).toBe(1);
      expect(mockDatabase.createUser).toHaveBeenCalledWith(userData);
    });

    it('should throw error for duplicate email', async () => {
      // Arrange
      const userData = { email: 'existing@example.com', name: 'Test' };
      mockDatabase.createUser.mockRejectedValue(
        new Error('Email already exists')
      );
      
      // Act & Assert
      await expect(userService.createUser(userData))
        .rejects.toThrow('Email already exists');
    });

    it('should validate email format', async () => {
      // Arrange
      const invalidData = { email: 'invalid-email', name: 'Test' };
      
      // Act & Assert
      await expect(userService.createUser(invalidData))
        .rejects.toThrow('Invalid email format');
    });
  });
});
```

**Important reminder:** While Copilot generates excellent test scaffolding and common test cases, you must still review the tests to ensure they validate your specific business logic correctly. Copilot understands common patterns but doesn't understand your unique business requirements.

### Example 4: Meta-Prompting (Let Copilot Write Your Prompts)

Here's an advanced technique that experienced Copilot users leverage: asking Copilot to help you write better prompts.

**The Concept:** Copilot knows what context it needs to generate better code. By asking it to write prompts, you learn to provide better instructions and ensure you don't miss critical details.

**Prompt:**
```
I need to ask Copilot to create a RESTful API with authentication. 
Write a detailed prompt that will help Copilot understand:
- Tech stack (Node.js/Express)
- Auth requirements (JWT with refresh tokens)
- Database schema
- Error handling standards
- API documentation needs
```

**Why this works:**
- Copilot knows its own capabilities and limitations
- You learn what information leads to better results
- Ensures consistency when generating similar code
- Helps codify team standards into reusable prompts

**Real-world applications:**
- Creating project templates for your team
- Establishing coding standards that Copilot can follow
- Documenting complex requirements before implementation
- Training junior developers on how to communicate technical requirements

### Example 5: Code Review Assistance

Code reviews are essential but time-consuming. Copilot can perform an initial review, catching common issues before human reviewers need to look at the code.

**Self-Review Prompt (Before Committing):**
```
Review this code for:
1. Bugs and logic errors
2. Performance issues
3. Security vulnerabilities
4. Best practice violations
5. Readability and maintainability
6. Missing error handling

Suggest specific improvements with code examples.
```

**PR Review Prompt:**
```
Analyze this pull request:
- Summarize the changes and intent
- Identify potential breaking changes
- Check for security issues
- Verify error handling
- Suggest test scenarios
- Rate the code quality (1-10) with justification
```

**Interactive workflow:**
1. Show Copilot your code (or paste the relevant section)
2. Ask for a review using the prompt above
3. Address identified issues
4. Re-submit for a second review to verify fixes

**Pro tip:** You can ask Copilot to generate a custom review checklist based on your team's specific coding standards:

```
Create a code review checklist for our team that includes:
- Our specific naming conventions
- Required documentation standards
- Security requirements for handling user data
- Performance benchmarks
- Accessibility requirements
```

---

### Example 6: Debugging with Context

When applications throw errors in production, quickly identifying the root cause is critical. Copilot excels at analyzing error messages and stack traces when provided with proper context.

**The Scenario:** Your application is throwing intermittent errors in production, and you need to quickly identify the root cause.

**Prompt for Stack Trace Analysis:**
```
Analyze this error and suggest fixes:

Error: Cannot read property 'user' of undefined
    at UserController.getProfile (src/controllers/user.js:45)
    at Layer.handle (node_modules/express/lib/router/layer.js:95)
    at next (node_modules/express/lib/router/route.js:137)

Context:
- Express API endpoint: GET /api/users/:id/profile
- Uses JWT authentication middleware
- MongoDB database
- Error occurs intermittently (about 5% of requests)

Provide:
1. Root cause analysis
2. Specific code fix
3. How to prevent similar issues
4. Suggested logging/monitoring improvements
```

**What Copilot can identify:**
- Race conditions or timing issues
- Missing null checks or validation
- Authentication/session handling problems
- Database query issues
- Async/await misuse

**Example Analysis Output:**
```
Root Cause: The error occurs when the JWT token is valid but the user
has been deleted from the database. The auth middleware sets req.user,
but the subsequent user lookup returns null.

Fix:
// Before (problematic):
async getProfile(req, res) {
  const user = await User.findById(req.user.id);
  return res.json({ email: user.email }); // Crashes if user is null
}

// After (fixed):
async getProfile(req, res) {
  const user = await User.findById(req.user.id);
  
  if (!user) {
    return res.status(404).json({ 
      error: 'User not found',
      code: 'USER_DELETED'
    });
  }
  
  return res.json({ email: user.email });
}

Prevention:
1. Add null checks after all database queries
2. Implement soft deletes instead of hard deletes
3. Add middleware to verify user exists after auth
4. Set up alerts for 404 spikes on authenticated endpoints
```

**Advanced Debugging Prompt:**
```
I'm seeing performance degradation in production.
Analyze this code and identify bottlenecks:

[paste code]

Metrics:
- Response time increased from 200ms to 3s
- CPU usage: 80%+
- Started after deploying feature X

Suggest optimizations with benchmark comparisons.
```

### Example 7: API Migration & Framework Upgrades

Migrating between framework versions or changing API patterns is tedious and error-prone. Copilot can automate much of this work while maintaining consistency.

**Scenario: Express 4 → Express 5 + TypeScript Migration**

**Prompt:**
```
Migrate this Express 4 JavaScript API to Express 5 with TypeScript:

Requirements:
1. Convert all routes to TypeScript with proper types
2. Update deprecated Express methods
3. Add request/response typing
4. Implement proper error handling middleware
5. Maintain backward compatibility with existing clients
6. Add input validation with express-validator

Current structure:
- 15 route files
- MongoDB with Mongoose
- JWT authentication
- REST endpoints

Provide migration plan and convert the UserController as example.
```

**What Copilot provides:**
```typescript
// Migration Plan:
// 1. Install dependencies (express@5, @types/express, typescript)
// 2. Create tsconfig.json
// 3. Convert controllers one-by-one
// 4. Update middleware for Express 5 changes
// 5. Test each endpoint

// Before (Express 4, JavaScript):
const express = require('express');
const router = express.Router();

router.get('/users/:id', async (req, res) => {
  try {
    const user = await User.findById(req.params.id);
    res.json(user);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// After (Express 5, TypeScript):
import { Router, Request, Response, NextFunction } from 'express';
import { body, param, validationResult } from 'express-validator';

interface UserParams {
  id: string;
}

interface UserResponse {
  id: string;
  email: string;
  name: string;
}

const router = Router();

router.get(
  '/users/:id',
  param('id').isMongoId().withMessage('Invalid user ID'),
  async (
    req: Request<UserParams>,
    res: Response<UserResponse>,
    next: NextFunction
  ): Promise<void> => {
    try {
      const errors = validationResult(req);
      if (!errors.isEmpty()) {
        res.status(400).json({ errors: errors.array() });
        return;
      }

      const user = await User.findById(req.params.id);
      
      if (!user) {
        res.status(404).json({ error: 'User not found' });
        return;
      }

      res.json({
        id: user.id,
        email: user.email,
        name: user.name
      });
    } catch (err) {
      next(err); // Express 5 improved error handling
    }
  }
);

export default router;
```

**Other migration scenarios Copilot handles well:**
- REST to GraphQL conversion
- jQuery to React/Vue/Angular
- Class components to React Hooks
- Upgrading authentication libraries (Passport, Auth0)
- Database ORM changes (Sequelize to Prisma, TypeORM to Drizzle)

**Prompt for Breaking Changes:**
```
Analyze this codebase for breaking changes when upgrading from 
[Library X v1] to [Library X v2]. Provide:
1. List of breaking changes affecting our code
2. Required code modifications
3. Migration script if needed
4. Testing strategy
```

### Example 8: Regex & Complex String Parsing

Regular expressions are notoriously difficult to write and debug. Copilot excels at generating, explaining, and testing regex patterns.

#### Use Case 1: Log File Parsing

**Prompt:**
```
Create a regex pattern and parser function to extract data from these logs:

Sample log format:
[2025-10-21 14:32:15] INFO [UserService] User login: user@aa.com (IP: 192.168.1.100) - Success
[2025-10-21 14:32:18] ERROR [PaymentService] Transaction failed: txn_123abc (amount: $49.99) - Insufficient funds
[2025-10-21 14:33:01] WARN [EmailService] Rate limit approaching: 950/1000 emails sent

Extract:
- Timestamp
- Log level
- Service name
- Message details
- Key-value pairs (user, IP, transaction ID, amount)

Return as structured JSON.
```

**Copilot Output:**
```javascript
const logPattern = /^\[(?<timestamp>[\d\-: ]+)\] (?<level>\w+) \[(?<service>\w+)\] (?<message>.+)$/;

function parseLogEntry(logLine) {
  const match = logLine.match(logPattern);
  if (!match) return null;

  const { timestamp, level, service, message } = match.groups;
  
  // Extract key-value pairs from message
  const kvPattern = /(\w+):\s*([^\s)]+)/g;
  const metadata = {};
  let kvMatch;
  
  while ((kvMatch = kvPattern.exec(message)) !== null) {
    metadata[kvMatch[1]] = kvMatch[2];
  }

  return {
    timestamp: new Date(timestamp),
    level,
    service,
    message: message.split(':')[0].trim(),
    metadata
  };
}

// Usage:
const log = '[2025-10-21 14:32:15] INFO [UserService] User login: user@aa.com (IP: 192.168.1.100)';
console.log(parseLogEntry(log));
// Output:
// {
//   timestamp: 2025-10-21T14:32:15.000Z,
//   level: 'INFO',
//   service: 'UserService',
//   message: 'User login',
//   metadata: { user: 'user@aa.com', IP: '192.168.1.100' }
// }
```

#### Use Case 2: Data Validation & Extraction

**Prompt:**
```
Create validation functions with regex for:
1. Airline booking reference (AA format: 6 alphanumeric characters)
2. Flight numbers (AA1234, AA 1234, AA-1234)
3. Frequent flyer numbers (AA + 7-9 digits)
4. International phone numbers (multiple formats)

Include test cases and error messages.
```

**Output Example:**
```javascript
/**
 * Validates AA booking reference format
 * @param {string} ref - Booking reference to validate
 * @returns {boolean} - True if valid
 */
function isValidBookingReference(ref) {
  const pattern = /^[A-Z0-9]{6}$/i;
  return pattern.test(ref);
}

/**
 * Extracts and normalizes flight number
 * @param {string} input - Flight number in any format
 * @returns {object|null} - { carrier: 'AA', number: '1234' } or null
 */
function parseFlightNumber(input) {
  const pattern = /^([A-Z]{2})[\s\-]?(\d{1,4})$/i;
  const match = input.match(pattern);
  
  if (!match) return null;
  
  return {
    carrier: match[1].toUpperCase(),
    number: match[2].padStart(4, '0'),
    formatted: `${match[1].toUpperCase()}${match[2]}`
  };
}

// Test cases
console.assert(isValidBookingReference('ABC123') === true);
console.assert(isValidBookingReference('abc123') === true);
console.assert(isValidBookingReference('ABC12') === false);

console.assert(parseFlightNumber('AA1234').formatted === 'AA1234');
console.assert(parseFlightNumber('AA 234').number === '0234');
console.assert(parseFlightNumber('AA-1234').carrier === 'AA');
```

**Pro Tip:** Ask Copilot to explain existing regex patterns:
```
Explain this regex in plain English and identify any issues:
/^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$/
```

Copilot Response: "This regex matches strings that are at least 8 characters long, contain at least one letter and one digit, but only allows letters and digits (no special characters). Issue: It doesn't enforce an uppercase letter requirement if that's needed for password complexity."

---

### Example 9: Environment Setup & Dockerization

Containerizing applications ensures consistent environments across development, testing, and production. Copilot can generate production-ready Docker configurations following security best practices.

**Scenario:** You need to containerize a Node.js/Express application for deployment.

**Basic Dockerization Prompt:**
```
Create Docker setup for this Node.js/Express application:

Requirements:
- Multi-stage build for optimization
- Non-root user for security
- Health check endpoint
- Environment variable configuration
- Development vs Production builds
- Docker Compose with PostgreSQL and Redis

Project structure:
- Node.js 20
- Express API
- PostgreSQL database
- Redis for caching
- Runs on port 3000
```

**Copilot Generated Files:**

**1. Dockerfile (Multi-stage, Production-Ready):**
```dockerfile
# Multi-stage build for optimized production image
FROM node:20-alpine AS builder

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production && \
    npm cache clean --force

# Copy application code
COPY . .

# Build step (if using TypeScript or build process)
RUN npm run build

# Production stage
FROM node:20-alpine

# Install dumb-init for proper signal handling
RUN apk add --no-cache dumb-init

# Create non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

# Set working directory
WORKDIR /app

# Copy dependencies from builder
COPY --from=builder --chown=nodejs:nodejs /app/node_modules ./node_modules
COPY --from=builder --chown=nodejs:nodejs /app/dist ./dist
COPY --from=builder --chown=nodejs:nodejs /app/package*.json ./

# Switch to non-root user
USER nodejs

# Expose port
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD node -e "require('http').get('http://localhost:3000/health', (r) => {process.exit(r.statusCode === 200 ? 0 : 1)})"

# Use dumb-init to handle signals properly
ENTRYPOINT ["dumb-init", "--"]

# Start application
CMD ["node", "dist/server.js"]
```

**2. docker-compose.yml (Complete Stack):**
```yaml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
      target: ${BUILD_TARGET:-production}
    ports:
      - "3000:3000"
    environment:
      NODE_ENV: ${NODE_ENV:-production}
      DATABASE_URL: postgres://user:password@postgres:5432/mydb
      REDIS_URL: redis://redis:6379
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_started
    networks:
      - app-network
    restart: unless-stopped

  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydb
    ports:
      - "5432:5432"
    volumes:
      - postgres-data:/var/lib/postgresql/data
      - ./init-db.sql:/docker-entrypoint-initdb.d/init.sql
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - app-network

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data
    command: redis-server --appendonly yes
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 3s
      retries: 5
    networks:
      - app-network

volumes:
  postgres-data:
  redis-data:

networks:
  app-network:
    driver: bridge
```

**3. .dockerignore (Optimize Build Context):**
```
node_modules
npm-debug.log
.env
.env.*
.git
.gitignore
README.md
.vscode
.idea
coverage
dist
*.md
```

**4. docker-compose.dev.yml (Development Override):**
```yaml
version: '3.8'

services:
  app:
    build:
      target: builder
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      NODE_ENV: development
    command: npm run dev
```

**Quick Start Commands:**
```bash
# Development
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up

# Production
docker-compose up -d

# View logs
docker-compose logs -f app

# Run migrations
docker-compose exec app npm run migrate

# Access database
docker-compose exec postgres psql -U user -d mydb
```

**Key Benefits of this Setup:**
- ✅ Consistent environments across team members
- ✅ Faster onboarding (eliminates "works on my machine" issues)
- ✅ Production-ready security practices (non-root user, health checks)
- ✅ Optimized image size with multi-stage builds
- ✅ Easy CI/CD integration
- ✅ Resource isolation and management

**Advanced Prompt for CI/CD Integration:**
```
Extend the Docker setup with:
1. GitHub Actions workflow for building and pushing to registry
2. Kubernetes deployment manifests (deployment, service, ingress)
3. Helm chart for configurable deployments
4. Security scanning (Trivy/Snyk)
5. Multi-architecture builds (amd64/arm64)

Registry: Azure Container Registry
Cluster: AKS (Azure Kubernetes Service)
```

**Why this matters:** Copilot automatically includes security best practices like non-root users, health checks, multi-stage builds, and proper signal handling – things that developers often overlook when manually creating Docker configurations.

---

## 3. Building Reusable Assets

The most efficient Copilot users don't start from scratch every time. They build libraries of prompts, instructions, and custom chat modes that encode their team's best practices and common workflows.

### Leveraging Community Resources

Before creating your own assets, explore what the community has already built. The **GitHub Awesome Copilot Repository** (https://github.com/github/awesome-copilot) is a curated collection of community-contributed instructions, prompts, and configurations.

You'll find:
- Industry-specific prompt templates
- Framework-specific instructions
- Custom chat modes for specialized tasks
- Best practices from experienced Copilot users

### Creating Custom Assets for Your Team

#### 1. Reusable Prompts (.prompt files)

Reusable prompts are pre-written prompt templates that you can invoke quickly without retyping common instructions.

**Example: create-readme.prompt**
```markdown
Create a professional README.md following this structure:
- Badge section (build status, coverage, version)
- Project description with clear value proposition
- Quick start (< 5 minutes to first result)
- Detailed installation
- Usage examples with code blocks
- API reference
- Contributing guidelines
- License

Tailor content based on detected language and framework.
```

**How to use:** Save this in a `.github/copilot/` directory in your project. When you need a README, you can reference this prompt or simply ask Copilot to "use the create-readme prompt."

**Other useful prompt templates:**
- API endpoint generation
- Database migration creation
- Test suite scaffolding
- Configuration file setup
- CI/CD pipeline generation

#### 2. Custom Instructions (.instructions.md files)

Instructions guide Copilot's behavior for specific domains, technologies, or patterns. They're like style guides that Copilot automatically follows.

**Example: playwright-python.instructions.md**
```markdown
# Playwright Python Testing Instructions

When generating Playwright tests:
- Use async/await syntax with pytest-playwright
- Include proper test isolation (browser context per test)
- Add explicit waits, avoid sleep()
- Use data-testid selectors over CSS/XPath
- Include screenshots on failure
- Follow Page Object Model pattern
- Add docstrings explaining test purpose

Example structure:
```python
import pytest
from playwright.async_api import Page, expect

@pytest.mark.asyncio
async def test_user_login(page: Page):
    """Verify user can login with valid credentials."""
    await page.goto("https://example.com/login")
    await page.fill('[data-testid="email"]', "user@test.com")
    await page.fill('[data-testid="password"]', "password123")
    await page.click('[data-testid="login-button"]')
    
    await expect(page).to_have_url("https://example.com/dashboard")
    await expect(page.locator('[data-testid="welcome"]')).to_be_visible()
```
```

**When to create instructions:**
- You have team-specific coding standards
- You work with a specialized framework or library
- You want to enforce security practices
- You have naming conventions or architectural patterns

**Example: api-design.instructions.md**
```markdown
# API Design Standards

When creating REST APIs:
- Use plural nouns for resource names (/users, not /user)
- Implement proper HTTP status codes:
  - 200: Success with body
  - 201: Created (return new resource)
  - 204: Success without body
  - 400: Client error with validation details
  - 401: Unauthorized
  - 403: Forbidden
  - 404: Not found
  - 500: Server error
- Include pagination for list endpoints (page, limit, total)
- Use snake_case for JSON keys
- Always validate input with detailed error messages
- Include request/response examples in comments
- Add OpenAPI/Swagger documentation
- Implement rate limiting headers
```

#### 3. Chat Modes (.chatmode.md files)

Chat modes allow you to create specialized versions of Copilot for specific tasks. Think of them as personas or roles that Copilot can assume.

**Example: prompt-engineer.chatmode.md**
```markdown
# Prompt Engineer Mode

You are an expert prompt engineer. When users describe a task:

1. Ask clarifying questions about:
   - Desired output format
   - Constraints and requirements
   - Context needed
   - Success criteria

2. Generate an optimized prompt that:
   - Provides clear context
   - Specifies output format
   - Includes examples
   - Sets appropriate constraints

3. Explain why the prompt is effective

Always provide both the prompt and reasoning.
```

**Use case:** Activate this mode when you need to craft an important prompt for a complex task. Instead of guessing, let Copilot help you formulate the best possible prompt.

**Other useful chat modes:**
- **Architect Mode**: For high-level design decisions
- **Security Auditor Mode**: Focused on security reviews
- **Performance Optimizer Mode**: For identifying bottlenecks
- **Documentation Writer Mode**: For creating comprehensive docs

### Setting Up Your Workspace

To maximize the benefit of custom assets, organize them systematically in your project.

**Recommended Structure:**
```
your-project/
├── .github/
│   └── copilot/
│       ├── prompts/
│       │   ├── create-readme.prompt
│       │   ├── generate-tests.prompt
│       │   └── code-review.prompt
│       ├── instructions/
│       │   ├── testing.instructions.md
│       │   ├── api-design.instructions.md
│       │   └── security.instructions.md
│       └── chatmodes/
│           ├── architect.chatmode.md
│           └── reviewer.chatmode.md
└── README.md
```

**Team Benefits:**
- **Consistency**: Everyone generates code following the same patterns
- **Onboarding**: New team members inherit best practices immediately
- **Quality**: Codified standards reduce review cycles
- **Efficiency**: Reduced prompt iteration time

**Getting started:**
1. Identify your team's most common tasks
2. Note the patterns you want to enforce
3. Create 2-3 instruction files for your most important standards
4. Build a small library of prompts for frequent tasks
5. Share with your team and iterate based on feedback

**Pro tip:** Version control your Copilot assets alongside your code. Treat them as part of your team's development infrastructure.

---

## 4. Best Practices & Tips

Mastering GitHub Copilot isn't just about knowing what prompts to write – it's about developing effective patterns for interaction, understanding when to use different modes, and maintaining security awareness. Modern AI coding tools have evolved from simple autocomplete into intelligent agents that can plan, build, test, and review code autonomously.

### Five AI-Assisted Coding Techniques That Save Time

#### 1. Let AI Read Your Design Docs Before You Code

One of the easiest ways to get better results from coding models is to stop giving them isolated prompts and start giving them context. When you share your design document, architecture overview, or feature specification before asking for code, you give the model a complete picture of what you're trying to build.

❌ **Weak prompt**: "Write a FastAPI endpoint for creating new users."

✅ **Context-rich prompt**:
```
You're helping implement the 'User Management' module described below.
The system uses JWT for auth, and a PostgreSQL database via SQLAlchemy.
Create a FastAPI endpoint for creating new users, validating input, and returning a token.

[Include your design doc, architecture overview, or AGENTS.md file]
```

**Why this works:**
- Responses align with your architecture, naming conventions, and data flow
- Less time spent rewriting or debugging mismatched code
- The model understands your system's constraints and patterns upfront

**For Copilot users:** Attach your design documents, architecture files, or coding standards to the chat before requesting implementations. Open related files in your editor to provide implicit context.

#### 2. Use One AI to Code, Another to Review

Every experienced team has two core roles: the builder and the reviewer. You can now reproduce that pattern with AI models acting in different capacities.

**The pattern:**
- One model (like Claude 3.5 Sonnet) acts as the code generator, producing the initial implementation
- A second perspective (switching models or using a fresh chat) reviews the diff, adds inline comments, and suggests corrections

**Example workflow:**
```
# First: Generate code
"Implement a caching layer with Redis for our user service"

# Second: Review the generated code
"Review the following code for performance, clarity, and edge cases:
[paste generated code]"
```

**Time savings:**
- The model can find its own logical errors
- Review feedback comes instantly with higher confidence
- Reduces human review overhead for routine or boilerplate updates
- Catches issues before they reach your team's code review

**In practice with Copilot:** Generate code in Agent mode, then open a new chat session and ask Copilot to review the implementation critically, looking for edge cases, security issues, and potential bugs.

#### 3. Automate Tests and Validation with AI

Writing tests isn't hard; it's just tedious. That's why it's one of the best areas to delegate to AI. Modern coding agents can now read your existing test suite, infer missing coverage, and generate new tests automatically.

**Common workflow:**
```
"Analyze the test coverage for UserService.js and generate tests for:
- Uncovered edge cases
- Error handling paths
- Input validation scenarios
- Mock setup for external dependencies

Use Jest and follow our existing test patterns in __tests__/ directory"
```

**The biggest time savings:**
- Letting AI fix failing tests during version bumps or refactors
- Generating comprehensive test suites for new features
- Identifying and testing edge cases you might have missed
- Keeping tests up-to-date as code evolves

**Results:**
- CI pipeline stays green with minimal human attention
- Tests stay current as your code evolves
- Catch regressions early without manually rewriting tests

#### 4. Use AI to Refactor and Modernize Legacy Code

Old codebases slow everyone down, not because they're bad, but because no one remembers why things were written that way. AI-assisted refactoring can bridge that gap by reading, understanding, and modernizing code safely and incrementally.

**Effective refactoring prompts:**
```
"Upgrade this project from React 17 to React 19:
- Adopt the new app directory structure
- Update deprecated APIs and lifecycle methods
- Ensure all existing tests still pass
- Provide a migration checklist for breaking changes"
```

**What AI agents can handle:**
- Upgrading dependencies and framework versions
- Rewriting modules in newer frameworks
- Converting classes to functions (or vice versa)
- Modernizing syntax and patterns
- Identifying and fixing deprecated API usage

**Best practice:** Use Agent mode for refactoring tasks that span multiple files. The agent can understand dependencies between files and make coordinated changes across your codebase.

#### 5. Work Asynchronously with Multiple AI Sessions

When you're deep in a coding session, waiting for model replies can break your flow. You can leverage Copilot's chat for multiple parallel tasks while staying focused on your main work.

**Parallel task approach:**
- Open multiple Copilot chat windows for different tasks
- Queue up distinct tasks: documentation, type definitions, validation, tests
- Continue working locally while AI processes each request
- Review and integrate results as they complete

**Example parallel tasks:**
```
# Chat 1: "Write TypeScript types for all API responses in api/responses"
# Chat 2: "Add input validation to the /signup route using Zod"
# Chat 3: "Document the auth middleware with JSDoc comments"
```

**This asynchronous approach saves time because:**
- Nothing blocks your workflow
- Results arrive incrementally
- You can review diffs and accept changes independently
- You stay in flow state on your primary task

### Putting It All Together: A Continuous Feedback-Driven Workflow

Each of these techniques works well on its own, but the real advantage comes from chaining them into a continuous workflow:

1. **Design-driven prompting**: Start with a well-structured spec or design doc. Feed it to Copilot as context so it knows your architecture, patterns, and constraints.

2. **Dual-perspective coding**: Generate code, then review it from a fresh perspective (new chat session) to validate logic, spot edge cases, and improve quality.

3. **Automated test generation**: Let Copilot create or repair tests as soon as new code lands, ensuring every change remains verifiable and CI/CD ready.

4. **AI-driven refactoring**: Use Agent mode to handle repetitive upgrades, dependency bumps, config migrations, and deprecated API rewrites in the background.

5. **Prompt evolution**: Feed back results from previous tasks—successes and mistakes alike—to refine your prompts over time. This is how AI workflows mature into reliable, semi-autonomous systems.

### When to Use Agent vs Ask Mode

Choosing the right mode significantly impacts efficiency. Here's a decision matrix:

| Task Type | Mode | Why |
|-----------|------|-----|
| Quick syntax question | **Ask** | Faster, no file access overhead |
| "How do I parse JSON in Python?" | **Ask** | Simple, contained answer |
| Refactoring multiple files | **Agent** | Needs to read and edit across files |
| Debugging complex issues | **Agent** | Requires codebase search and analysis |
| Explaining a concept | **Ask** | Straightforward, no context needed |
| Generating boilerplate project | **Agent** | Creates multiple related files |
| Code snippet for specific function | **Ask** | Self-contained, single response |
| "Fix all TypeScript errors in this project" | **Agent** | Needs to scan and fix systematically |
| API usage example | **Ask** | Direct, focused example |
| Architecture review | **Agent** | Needs to analyze entire codebase |

**General rule**: If the task requires understanding relationships between files or making coordinated changes across your codebase, use **Agent mode**. If you need a quick answer to a specific question, use **Ask mode**.

### Security Considerations

GitHub Copilot is powerful, but with power comes responsibility. Always keep these security principles in mind:

#### Review All Generated Code

Never blindly accept Copilot's suggestions, especially for:
- Authentication and authorization logic
- Input validation and sanitization
- Cryptographic operations
- Database queries (SQL injection risks)
- File system operations
- API key handling

**Best practice**: Treat Copilot's output like code from a junior developer – it might be excellent, but it always needs review.

#### Never Share Sensitive Data in Prompts

❌ **Don't do this:**
```
Here's our production database connection string: 
postgresql://admin:SecretPass123@prod-db.company.com:5432/maindb
Help me optimize this connection.
```

✅ **Do this instead:**
```
Help me optimize this PostgreSQL connection configuration:
- Connection pooling parameters
- Timeout settings
- SSL/TLS requirements
- Best practices for production environments
```

**Remember**: Your prompts may be used to improve the model. Never include:
- API keys or secrets
- Passwords
- Connection strings with credentials
- Personally identifiable information (PII)
- Proprietary business logic that's confidential

#### Validate Security-Critical Code

For security-sensitive code, use a multi-step approach:

1. **Generate**: Ask Copilot to create the implementation
2. **Review**: Manually audit the security aspects
3. **Verify**: Ask Copilot to review its own code for security issues
4. **Test**: Run security scanning tools (SonarQube, Snyk, etc.)
5. **Peer Review**: Have a team member review before deploying

**Example security review prompt:**
```
Review this authentication code for security vulnerabilities:
- SQL injection risks
- XSS vulnerabilities  
- CSRF protection
- Password storage (hashing, salting)
- Session management
- Rate limiting
- Input validation

[paste code]
```

#### Understand Licensing

Copilot learns from public code, which means generated code might resemble existing open-source projects.

**Best practices:**
- Review generated code for uncommon patterns that might be copied
- For critical components, verify the approach is standard/generic
- Understand your organization's policies on AI-generated code
- When in doubt, refactor to ensure originality

### Performance Tips

**Faster iteration:**
- Keep your prompts concise but complete
- Use Ask mode for quick questions to avoid Agent overhead
- Build a prompt library to avoid retyping
- Use keyboard shortcuts for Copilot in your IDE

**Better results:**
- Reference files explicitly: "In UserService.js, add a method..."
- Specify your testing framework, linting rules, and style preferences
- Include error messages when debugging
- Mention performance requirements upfront

**Context management:**
- Close unnecessary files to reduce noise in Agent mode
- Open related files before making a complex request
- Clear chat history if Copilot seems confused by old context

### Common Pitfalls to Avoid

1. **Over-reliance**: Don't stop thinking critically. Copilot amplifies your expertise but isn't a replacement for engineering judgment.

2. **Ignoring edge cases**: AI typically generates happy-path code. You must still think about edge cases, error conditions, and boundary scenarios.

3. **Skipping tests**: Fast code generation doesn't mean you should skip testing. Always validate AI-generated code with comprehensive tests.

4. **Copy-paste without understanding**: Always understand the code before integrating it. Review logic, dependencies, and potential impacts.

5. **Not updating instructions**: As your team's standards evolve, update your custom instructions and prompt library to match.

6. **Exposing sensitive data**: Never include API keys, passwords, connection strings, or PII in your prompts. Your prompts may be used to improve the model.

---

## 5. Next Steps & Resources

Now that you understand Copilot's capabilities, the next step is consistent practice and integration into your workflow.

### Hands-On Practice

Use Copilot to:

#### Create Documentation
- **Generate a README.md** for your project
  - Analyze the codebase to understand tech stack
  - Include installation, usage, and API documentation
  - Add contributing guidelines and license information

#### Project Assessment
- **Perform a project assessment**
  - Analyze architecture, dependencies, and code quality
  - Identify technical debt and security concerns
  - Create *hangar-improvement* stories in ADO based on findings

#### Establish Code Standards
- **Create a `code standard` document**
  - Save it as a markdown file in your project
  - Use Copilot to enforce these standards in the IDE
  - Implement pre-commit hooks to check standards
  - Integrate standard checks during build process

#### Generate and Review Tests
- **Generate tests for one module**
  - Target modules with low test coverage
  - Review generated tests for business logic accuracy
  - Run tests and fix any issues
  - Identify patterns that work well for your codebase

#### Build Custom Prompts
- **Create custom prompts for your common tasks**
  - Create a `prompts` folder in your project's `.github` directory
  - Save prompts that work well for your team
  - Share successful patterns with colleagues
  - Iterate based on real usage and feedback

#### Refactoring with Agent Mode
- **Use Agent mode for refactoring**
  - Address issues identified in project assessment
  - Tackle technical debt found by CodeScene
  - Use multi-file context for complex refactoring
  - Validate changes with comprehensive tests

### Additional Resources

#### Official Documentation
- **GitHub Copilot Docs**: https://docs.github.com/copilot  
  Comprehensive official documentation covering all features and capabilities

#### Community Resources
- **Awesome Copilot**: https://github.com/github/awesome-copilot  
  Community-contributed instructions, prompts, and best practices
- **Community Patterns**: Browse the awesome-copilot repo for inspiration and advanced techniques
- **5 AI-Assisted Coding Techniques**: https://www.kdnuggets.com/5-ai-assisted-coding-techniques-guaranteed-to-save-you-time  
  Practical techniques for saving time with AI coding tools

#### Internal Resources
- **Internal AA License**: https://developer.aa.com/access/github-copilot  
  Access to licenses and internal documentation

### Questions to Explore

As you continue your Copilot journey, consider these questions:

**For Your Workflow:**
- How can Copilot help with your specific tech stack?
- What repetitive tasks could you automate with better prompts?
- Which team coding standards could become Copilot instructions?

---

## Appendix: Quick Reference

### Prompt Templates Cheat Sheet

Keep these templates handy for common tasks:

#### Documentation Generation
```
Create comprehensive documentation for [file/module/project]:
- Purpose and overview
- Installation/setup instructions  
- Public API reference with parameter descriptions
- Usage examples with code blocks
- Edge cases and limitations
- Contributing guidelines (if applicable)

Analyze the code to ensure accuracy.
```

#### Code Review
```
Review this code for:
1. Bugs and logic errors
2. Security vulnerabilities (injection, XSS, auth issues)
3. Performance bottlenecks
4. Best practice violations
5. Readability and maintainability issues
6. Missing error handling or edge cases

Provide specific fixes with code examples for each issue found.
```

#### Test Generation
```
Generate comprehensive [unit/integration/e2e] tests for [component/module]:
- Cover happy path scenarios
- Include edge cases and boundary conditions
- Test error handling and invalid inputs
- Mock external dependencies appropriately
- Use [Jest/Pytest/JUnit - your framework]
- Follow [AAA/Given-When-Then] pattern
- Aim for [80%+] code coverage

Include setup/teardown and test data fixtures.
```

#### Refactoring
```
Refactor this code to:
1. [Specific improvement - e.g., "Extract business logic into service layer"]
2. [Another improvement - e.g., "Add TypeScript types"]
3. [Performance goal - e.g., "Reduce database queries"]
4. [Pattern to apply - e.g., "Implement Repository pattern"]

Maintain current functionality exactly - do not change behavior.
Add tests to verify equivalence before and after refactoring.
```

#### Debugging
```
Analyze this error and suggest fixes:

Error: [paste error message and stack trace]

Context:
- [What the code is trying to do]
- [Tech stack and frameworks]
- [When the error occurs - consistently or intermittently]
- [Recent changes that might be related]

Provide:
1. Root cause analysis
2. Specific code fix with before/after comparison
3. How to prevent similar issues
4. Suggested logging or monitoring improvements
```

#### Migration
```
Migrate this code from [Source] to [Target]:

Requirements:
- Maintain all existing functionality
- Update deprecated methods/patterns
- Add proper types/interfaces (if applicable)
- Improve error handling
- Follow [Target] best practices
- Include migration notes for any breaking changes

Provide step-by-step migration plan before implementation.
```

### Keyboard Shortcuts

Speed up your workflow with these shortcuts (VS Code):

| Action | Windows/Linux | Mac |
|--------|---------------|-----|
| Open Copilot Chat | `Ctrl+Alt+I` | `Cmd+Option+I` |
| Accept suggestion | `Tab` | `Tab` |
| Reject suggestion | `Esc` | `Esc` |
| Next suggestion | `Alt+]` | `Option+]` |
| Previous suggestion | `Alt+[` | `Option+[` |
| Trigger inline suggestion | `Alt+\` | `Option+\` |

### When Copilot Doesn't Work Well

Copilot has limitations. It typically struggles with:

**Highly specialized domains:**
- Proprietary internal frameworks
- Niche scientific computations
- Very new technologies (released after training data cutoff)

**Business logic:**
- Company-specific rules and workflows
- Domain-specific calculations
- Unique regulatory requirements

**Architecture decisions:**
- System design tradeoffs
- Technology selection
- Scalability planning

**Solution**: For these scenarios, provide extensive context, examples, and explicit requirements. Or, make the high-level decisions yourself and use Copilot for implementation details.

---

## Conclusion

GitHub Copilot represents a fundamental shift in how developers work. It's not about replacing human creativity and judgment – it's about amplifying your capabilities and eliminating tedious work that slows you down.

**Key takeaways:**
- **Start small**: Begin with documentation and tests before tackling complex refactoring
- **Be specific**: Detailed prompts produce better results
- **Build assets**: Create reusable prompts and instructions for your team
- **Stay secure**: Always review generated code, especially for security-critical functions
- **Iterate**: Refine prompts based on what works for your specific use cases
- **Share knowledge**: Help your team adopt Copilot effectively

The developers who get the most value from Copilot are those who:
1. Understand when and how to use different modes
2. Craft clear, context-rich prompts
3. Critically evaluate all generated code
4. Build libraries of effective patterns
5. Continuously refine their approach

Remember: Copilot is a tool that amplifies your expertise. The better developer you are, the more value you'll extract from it. Use it to eliminate repetitive work so you can focus on the creative, high-impact aspects of software engineering.

**Now it's your turn.** Start with Week 1 of the practice plan, generate your first README, and begin your journey to mastering GitHub Copilot.

---

*Last updated: October 21, 2025*  
*For questions or feedback, contact your development team or visit the internal developer portal.*

