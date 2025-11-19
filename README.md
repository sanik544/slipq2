# SLIP 10 - Question 2: Complete CI/CD Pipeline

This folder contains a sample application for SLIP 10, Question 2 (Complete CI/CD Pipeline using Jenkins Declarative Syntax).

## Overview

This Node.js application demonstrates a complete CI/CD pipeline with:
- **Checkout**: Git clone
- **Build**: Compile/build application
- **Test**: Run unit, integration, and E2E tests in parallel
- **Quality Gate**: Check test results and quality metrics
- **Package**: Create deployable artifacts
- **Deploy**: Deploy to different environments
- **Post-Build Actions**: Archive artifacts, notifications, cleanup

## Application Structure

```
Q2/
├── src/
│   ├── index.js           # Express API server
│   └── orderService.js    # Business logic (Order management)
├── __tests__/
│   ├── unit/
│   │   └── orderService.test.js    # Unit tests
│   ├── integration/
│   │   └── api.test.js             # Integration tests
│   └── e2e/
│       └── workflow.test.js        # E2E tests
├── scripts/
│   └── package.js         # Packaging script
├── package.json           # Dependencies and scripts
├── Jenkinsfile            # Complete CI/CD pipeline
├── .eslintrc.js          # ESLint configuration
├── .gitignore            # Git ignore file
└── README.md             # This file
```

## Prerequisites

- Node.js (v14 or higher)
- npm (Node Package Manager)
- Jenkins (for pipeline execution)
- Git (for version control)

## Installation

1. Navigate to the Q2 directory:
```bash
cd SLIP_10/Q2
```

2. Install dependencies:
```bash
npm install
```

## Running Tests

### Run all tests:
```bash
npm test
```

### Run unit tests only:
```bash
npm run test:unit
```

### Run integration tests only:
```bash
npm run test:integration
```

### Run E2E tests only:
```bash
npm run test:e2e
```

### Run tests with coverage:
```bash
npm run test:coverage
```

## Pipeline Stages

### 1. Checkout
- Clones repository from Git
- Shows Git version and commit information
- Uses parameters for version tracking

### 2. Build
- Installs npm dependencies
- Executes build script
- Prepares application for testing

### 3. Test (Parallel Execution)
Three parallel branches execute simultaneously:

**a) Unit Tests**
- Fast, isolated tests
- Tests business logic in `orderService.js`
- Typically < 5 seconds

**b) Integration Tests**
- End-to-end API tests
- Tests HTTP endpoints
- Tests complete application flow

**c) E2E Tests**
- Full workflow tests
- Tests complete user journeys
- Tests multiple operations together

### 4. Quality Gate
- Checks test results
- Generates test coverage reports
- Runs quality checks (linting, security audit)
- Publishes test results

### 5. Package
- Creates deployable artifacts in `dist/` directory
- Copies source files and configuration
- Prepares artifacts for deployment

### 6. Deploy
- Deploys to environment based on `DEPLOY_ENV` parameter
- Supports dev, staging, and prod environments
- Copies artifacts to deployment directory

## Pipeline Parameters

### VERSION (String)
- **Default**: `1.0.0`
- **Description**: Application version number
- **Usage**: Used in build and deployment process

### DEPLOY_ENV (Choice)
- **Options**: `dev`, `staging`, `prod`
- **Description**: Deployment environment
- **Usage**: Determines deployment target and configuration

## Expected Pipeline Flow

```
Checkout
   ↓
Build
   ↓
Test (Parallel)
   ├─→ Unit Tests ─┐
   ├─→ Integration Tests ─┤ All must complete
   └─→ E2E Tests ─┘
           ↓
   Quality Gate
           ↓
      Package
           ↓
      Deploy (based on DEPLOY_ENV)
           ↓
   Post-Build Actions
```

## Pipeline Execution Examples

### Example 1: Development Deployment
**Parameters:**
- VERSION: `1.0.0`
- DEPLOY_ENV: `dev`

**Result:**
- All tests run in parallel
- Quality gate passes
- Package created
- Deployed to development environment

### Example 2: Production Deployment
**Parameters:**
- VERSION: `2.0.0`
- DEPLOY_ENV: `prod`

**Result:**
- All tests run in parallel
- Quality gate passes
- Package created
- Deployed to production environment

## Post-Build Actions

### Always:
- Displays build summary
- Shows parameters used
- Shows build duration

### Success:
- Archives build artifacts
- Archives package.json
- Archives test coverage reports
- Archives test results

### Failure:
- Logs error information
- Triggers email notification (if configured)
- Provides build URL for troubleshooting

### Cleanup:
- Performs workspace cleanup
- Removes temporary files

## Test Coverage

### Unit Tests (`__tests__/unit/orderService.test.js`)
- **createOrder()**: 4 test cases
- **getOrderById()**: 2 test cases
- **updateOrder()**: 2 test cases
- **deleteOrder()**: 1 test case

**Total: 9 unit test cases**

### Integration Tests (`__tests__/integration/api.test.js`)
- Health check endpoint
- Complete CRUD operations
- Error handling tests

**Total: 6+ integration test cases**

### E2E Tests (`__tests__/e2e/workflow.test.js`)
- Complete order lifecycle
- Multiple orders workflow

**Total: 2+ E2E test cases**

## Pipeline Visualization

In Jenkins Blue Ocean or classic view, you will see:
- Sequential stages: Checkout, Build, Quality Gate, Package, Deploy
- Parallel stage: Test (with 3 parallel branches)
- Clear visualization of execution flow
- Stage execution times
- Test results and coverage

## Running the Application

Start the Express server:
```bash
npm start
```

The API will be available at `http://localhost:3000`

### API Endpoints

- `GET /` - API information
- `GET /health` - Health check endpoint
- `GET /api/orders` - Get all orders
- `GET /api/orders/:id` - Get order by ID
- `POST /api/orders` - Create a new order
- `PUT /api/orders/:id` - Update an order
- `DELETE /api/orders/:id` - Delete an order

## Troubleshooting

### Tests fail in parallel execution
- Check test output in Jenkins console
- Verify all dependencies are installed
- Run tests locally: `npm test`

### Quality gate fails
- Review test results
- Check test coverage reports
- Fix linting issues: `npm run lint`

### Deployment fails
- Verify deployment directory permissions
- Check deployment script execution
- Review deployment logs

### Artifacts not archived
- Check artifact paths in Jenkinsfile
- Verify files exist in workspace
- Check Jenkins workspace permissions

## Complete Workflow Demonstration

1. **Code Commit**: Developer commits code to Git repository
2. **Checkout**: Jenkins clones repository
3. **Build**: Application dependencies installed and built
4. **Test**: All tests executed in parallel
5. **Quality Gate**: Test results and quality metrics checked
6. **Package**: Deployable artifacts created
7. **Deploy**: Artifacts deployed to specified environment
8. **Archive**: Build artifacts archived for later use
9. **Notification**: Status communicated (success/failure)

## Additional Resources

- [Jest Documentation](https://jestjs.io/docs/getting-started)
- [Jenkins Pipeline Documentation](https://www.jenkins.io/doc/book/pipeline/)
- [Jenkins Parallel Stages](https://www.jenkins.io/doc/book/pipeline/syntax/#parallel)
- [Express.js Documentation](https://expressjs.com/)

## Notes for Evaluators

- Pipeline demonstrates complete CI/CD workflow
- All required stages implemented
- Parallel test execution clearly shown
- Quality gate checks test results
- Package and deploy stages functional
- Post-build actions include archive, notifications, and cleanup
- Pipeline parameters for version and environment
- Ready for end-to-end demonstration

