# GitHub Copilot: Beyond the Basics
Introduction to Advanced Copilot Features for Developers

**Get License**: https://developer.aa.com/access/github-copilot

---

## 1. Getting Started: IDE Integration
**Duration**: 5-7 minutes

### Supported IDEs
* **VS Code** - Most feature-rich integration
* **Visual Studio** - Full support for .NET developers  
* **IntelliJ IDEA** - Java/Kotlin optimized experience
* **Other IDEs**: Neovim, JetBrains suite

### Understanding Chat Modes
**Key Concept**: Ask vs Agent Mode

#### Ask Mode (Quick Questions)
* Single-turn conversations
* Fast, focused answers
* Best for: syntax questions, quick explanations, code snippets
* **Example**: "How do I parse JSON in Python?"

#### Agent Mode (Autonomous Tasks)
* Multi-step problem solving
* Can read files, search codebase, make edits
* Best for: refactoring, debugging, complex implementations
* **Example**: "Refactor this API to use async/await and add error handling"

**Demo Tip**: Show side-by-side comparison of same question in both modes

### Model Selection
* **Default**: Claude Sonnet 4.x
* **Alternative**: GPT-4.1 (for specific use cases)
* **When to switch**: Different models excel at different tasks
  - Sonnet: Better at code understanding and complex reasoning
  - GPT-4o: Sometimes better at creative solutions

**Speaker Note**: Mention that model availability may vary based on organization settings

---

## 2. Practical Examples & Use Cases
**Duration**: 15-20 minutes

### Example 1: Generate Professional README Files
**Scenario**: You have a project without documentation

**Prompt Template**:
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

**Demo Steps**:
1. Open a project without a README
2. Use Agent mode with the prompt above
3. Show how Copilot analyzes files to understand the project
4. Review and refine the generated content

**Key Takeaway**: Copilot can analyze your entire codebase for context

---

### Example 2: Project Assessment & Technical Debt Analysis
**Scenario**: Inheriting a legacy codebase or planning a refactor

**Prompt Template**:
```
Analyze this codebase and provide:
1. Architecture overview (patterns, structure)
2. Technology stack and dependencies
3. Code quality assessment (maintainability, test coverage)
4. Security concerns or vulnerabilities
5. Technical debt areas
6. Recommended improvements prioritized by impact
```

**What to Expect**:
- Dependency analysis (outdated packages, security issues)
- Code smell identification
- Architecture pattern recognition
- Actionable recommendations

**Demo Tip**: Use a small, real internal project for authenticity

---

### Example 3: Automated Test Generation
**Scenario**: Increasing test coverage for existing code

**Basic Prompt**:
```
Generate unit tests for the `UserService` class. Include:
- Happy path scenarios
- Edge cases
- Error handling
- Mock external dependencies
Use [Jest/NUnit/JUnit - specify your framework]
```

**Advanced Prompt**:
```
Create comprehensive test suite for this module:
1. Unit tests with 80%+ coverage
2. Integration tests for API endpoints
3. Test data factories/fixtures
4. Mock setup for database and external APIs
5. Follow AAA pattern (Arrange, Act, Assert)
```

**Real Example Output**:
```javascript
// Copilot can generate:
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
      const userData = { email: 'test@example.com', name: 'Test' };
      mockDatabase.createUser.mockResolvedValue({ id: 1, ...userData });
      
      // Act
      const result = await userService.createUser(userData);
      
      // Assert
      expect(result.id).toBe(1);
      expect(mockDatabase.createUser).toHaveBeenCalledWith(userData);
    });

    it('should throw error for duplicate email', async () => {
      // Test implementation...
    });
  });
});
```

**Speaker Note**: Emphasize that tests still need human review for business logic validation

---

### Example 4: Meta-Prompting (Copilot Writes Prompts)
**Advanced Technique**: Let Copilot help you write better prompts

**Prompt**:
```
I need to ask Copilot to create a RESTful API with authentication. 
Write a detailed prompt that will help Copilot understand:
- Tech stack (Node.js/Express)
- Auth requirements (JWT with refresh tokens)
- Database schema
- Error handling standards
- API documentation needs
```

**Why This Works**:
- Copilot knows what context it needs
- Ensures you don't miss important details
- Teaches you better prompting techniques

**Real-World Application**:
- Creating project templates
- Establishing team coding standards
- Documenting complex requirements

---

### Example 5: Code Review Assistance
**Scenario**: Reviewing PRs or your own code before commit

**Self-Review Prompt**:
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

**PR Review Prompt**:
```
Analyze this pull request:
- Summarize the changes and intent
- Identify potential breaking changes
- Check for security issues
- Verify error handling
- Suggest test scenarios
- Rate the code quality (1-10) with justification
```

**Interactive Demo**:
1. Show a code snippet with intentional issues (SQL injection, memory leak, etc.)
2. Use Copilot to identify issues
3. Ask for fix suggestions
4. Implement and re-review

**Advanced Tip**: Use Copilot to generate a review checklist customized for your team's standards

---

### Example 6: Debugging with Context
**Scenario**: Application throwing errors in production; need to quickly identify root cause

**Prompt for Stack Trace Analysis**:
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

**What Copilot Can Identify**:
- Race conditions or timing issues
- Missing null checks or validation
- Authentication/session handling problems
- Database query issues
- Async/await misuse

**Real-World Example Output**:
```
Root Cause: The error occurs when JWT token is valid but the user
has been deleted from the database. The auth middleware sets req.user,
but the user lookup returns null.

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

**Advanced Debugging Prompt**:
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

**Demo Tip**: Use a real error from your logs (sanitized) for authenticity

---

### Example 7: API Migration & Framework Upgrades
**Scenario**: Migrating from older framework version or changing API patterns

**Example: Express 4 → Express 5 + TypeScript Migration**

**Prompt**:
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

**What Copilot Provides**:
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

**Additional Migration Scenarios**:
- REST to GraphQL conversion
- jQuery to React/Vue
- Class components to React Hooks
- Upgrading authentication libraries (Passport, Auth0)
- Database ORM changes (Sequelize to Prisma)

**Prompt for Breaking Changes**:
```
Analyze this codebase for breaking changes when upgrading from 
[Library X v1] to [Library X v2]. Provide:
1. List of breaking changes affecting our code
2. Required code modifications
3. Migration script if needed
4. Testing strategy
```

---

### Example 8: Regex & Complex String Parsing
**Scenario**: Need to validate, extract, or transform complex string patterns

**Common Use Cases**:

#### 1. Log File Parsing
**Prompt**:
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

**Copilot Output**:
```javascript
const logPattern = /^\[(?<timestamp>[\d\-: ]+)\] (?<level>\w+) \[(?<service>\w+)\] (?<message>.+)$/;

function parseLogEntry(logLine) {
  const match = logLine.match(logPattern);
  if (!match) return null;

  const { timestamp, level, service, message } = match.groups;
  
  // Extract key-value pairs from message
  const kvPattern = /(\w+):\s*([^\s]+)/g;
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

#### 2. Data Validation & Extraction
**Prompt**:
```
Create validation functions with regex for:
1. Airline booking reference (AA format: 6 alphanumeric characters)
2. Flight numbers (AA1234, AA 1234, AA-1234)
3. Frequent flyer numbers (AA + 7-9 digits)
4. International phone numbers (multiple formats)

Include test cases and error messages.
```

**Output Example**:
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

#### 3. Text Transformation
**Prompt**:
```
Convert between naming conventions:
- camelCase ↔ snake_case ↔ kebab-case ↔ PascalCase

Handle edge cases:
- Acronyms (XMLParser, HTTPSConnection)
- Numbers (user2FA, item1Price)
- Special characters

Include bidirectional conversion and tests.
```

**Why Regex Examples Matter**:
- Regex is notoriously difficult to write and debug
- Copilot can explain complex patterns in plain English
- Instant test case generation
- Handles edge cases developers often miss

**Pro Tip**: Ask Copilot to explain existing regex patterns:
```
Explain this regex in plain English and identify any issues:
/^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$/
```

---

### Example 9: Environment Setup & Dockerization
**Scenario**: Containerizing an application for consistent dev/prod environments

**Basic Dockerization Prompt**:
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

**Copilot Generated Files**:

**1. Dockerfile**:
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

**2. docker-compose.yml**:
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

**3. .dockerignore**:
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

**4. Development Override**:
**docker-compose.dev.yml**:
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

**Advanced Prompt for CI/CD Integration**:
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

**What You Get**:
```yaml
# .github/workflows/docker-build.yml
name: Build and Push Docker Image

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

env:
  REGISTRY: myregistry.azurecr.io
  IMAGE_NAME: my-app

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Log in to Azure Container Registry
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ secrets.ACR_USERNAME }}
          password: ${{ secrets.ACR_PASSWORD }}

      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
          tags: |
            type=ref,event=branch
            type=ref,event=pr
            type=semver,pattern={{version}}
            type=sha

      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: .
          platforms: linux/amd64,linux/arm64
          push: ${{ github.event_name != 'pull_request' }}
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
          cache-from: type=gha
          cache-to: type=gha,mode=max

      - name: Run Trivy security scan
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}
          format: 'sarif'
          output: 'trivy-results.sarif'

      - name: Upload Trivy results
        uses: github/codeql-action/upload-sarif@v2
        with:
          sarif_file: 'trivy-results.sarif'
```

**Quick Start Commands**:
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

**Key Benefits**:
- ✅ Consistent environments across team
- ✅ Faster onboarding (no "works on my machine")
- ✅ Production-ready security practices
- ✅ Easy CI/CD integration
- ✅ Resource isolation and management

**Speaker Note**: Emphasize how Copilot handles security best practices (non-root user, health checks, multi-stage builds) automatically

---

## 3. Building Reusable Assets
**Duration**: 10 minutes

### Leveraging Community Resources
**GitHub Awesome Copilot Repository**  
🔗 https://github.com/github/awesome-copilot

Community-contributed instructions, prompts, and configurations

### Creating Custom Assets

#### 1. **Reusable Prompts** (.prompt files)
**Example**: `create-readme.prompt`
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

**How to Use**: Save in `.github/copilot/` directory for project-wide access

---

#### 2. **Custom Instructions** (.instructions.md files)
**Example**: `playwright-python.instructions.md`
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
    # ... rest of test
```
```

**Speaker Note**: Instructions guide Copilot's behavior for specific domains

---

#### 3. **Chat Modes** (.chatmode.md files)
**Example**: `prompt-engineer.chatmode.md`
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

**Use Case**: Activate this mode when crafting important prompts

---

### Setting Up Your Workspace

**Recommended Structure**:
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
│       │   └── api-design.instructions.md
│       └── chatmodes/
│           └── architect.chatmode.md
└── README.md
```

**Team Benefits**:
- Consistent code generation across team
- Codified best practices
- Faster onboarding
- Reduced prompt iteration time

---

## 4. Best Practices & Tips
**Duration**: 5 minutes

### Prompt Engineering Essentials

#### Be Specific
❌ **Vague**: "Make this better"  
✅ **Specific**: "Refactor this function to use async/await, add input validation, and include JSDoc comments"

#### Provide Context
```
I'm working on a Node.js microservice that processes payment webhooks.
The current error handling doesn't distinguish between retryable and 
permanent failures. Update the code to:
- Retry transient errors (network, timeout) with exponential backoff
- Log permanent errors (auth, validation) and alert
- Use a dead letter queue for failed attempts after 3 retries
```

#### Iterate and Refine
1. Start with a broad request
2. Review the output
3. Ask for specific adjustments
4. Build on previous responses

### When to Use Agent vs Ask Mode

| Task Type | Recommended Mode | Why |
|-----------|-----------------|-----|
| Quick syntax question | Ask | Faster, no file access needed |
| Refactoring multiple files | Agent | Needs codebase context |
| Explaining a concept | Ask | Straightforward, single response |
| Debugging complex issue | Agent | Can search and analyze code |
| Generating boilerplate | Agent | Can create multiple files |
| Code snippet | Ask | Simple, contained response |

### Security Considerations
⚠️ **Important Reminders**:
- Review all generated code before committing
- Don't share sensitive data in prompts (API keys, credentials)
- Validate security-critical code (auth, encryption, input validation)
- Test generated code thoroughly
- Copilot learns from public code - verify licensing

---

## 5. Next Steps & Resources

### Hands-On Practice

Use Copilot to:
* Create a README.md 
* Perform a project assessment - create *hangar-improvement* stories in ADO
* Create a `code standard` and save it as a markdown file in the project. 
  - Use Copilot to enforce these standards in the IDE, before commit and during build
* Generate and review tests for one module
* Create custom prompts for your common tasks
  - Create a `prompts` folder in your project `.github`
  - Save prompts that work well for your team
  - Share successful patterns with colleagues
  - Iterate based on real usage
* Use Agent mode to refactoring issue identified in project assessment or CodeScene

### Additional Resources
- **GitHub Copilot Docs**: https://docs.github.com/copilot
- **Awesome Copilot**: https://github.com/github/awesome-copilot
- **Internal AA License**: https://developer.aa.com/access/github-copilot
- **Community Patterns**: Browse the awesome-copilot repo for inspiration

### Questions to Explore
- How can Copilot help with your specific tech stack?
- What repetitive tasks could you automate with better prompts?
- Which team coding standards could become Copilot instructions?

---

## Appendix: Quick Reference

### Prompt Templates Cheat Sheet

**Documentation Generation**:
```
Create comprehensive documentation for [file/module]:
- Purpose and overview
- Public API reference
- Usage examples
- Edge cases and limitations
```

**Code Review**:
```
Review this code for: bugs, security issues, performance, 
best practices. Provide specific fixes with code examples.
```

**Test Generation**:
```
Generate [unit/integration] tests for [component]:
- Cover happy path and edge cases
- Mock dependencies
- Use [testing framework]
- Aim for [coverage]% coverage
```

**Refactoring**:
```
Refactor this code to:
1. [specific improvement]
2. [another improvement]
Maintain current functionality and add tests.
```

---

**Presentation Notes**:
- **Total Duration**: ~40-45 minutes + Q&A
- **Interactive Elements**: 3-4 live demos
- **Engagement**: Poll audience on current Copilot usage at start
- **Takeaway**: Provide this document + example prompt library
