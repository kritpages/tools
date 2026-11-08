# Tools

A collection of independent, lightweight web applications hosted using GitHub Pages.

Each application lives in its own directory and is deployed automatically through a shared GitHub Actions workflow.

## Repository Structure

```text
tools/
├── .github/
│   └── workflows/
│       └── deploy-pages.yml
├── pocketledger/
│   ├── assets/
│   └── index.html
├── app-two/
│   ├── assets/
│   └── index.html
└── README.md
```

## Hosting

Applications are hosted using GitHub Pages.

**Base URL:**

```text
https://<organization>.github.io/tools/
```

Each application is accessible using its directory name.

| Application | URL |
|---|---|
| PocketLedger | `/pocketledger/` |
| App Two | `/app-two/` |
| App Three | `/app-three/` |

## Onboarding a New Application

Follow these steps to add an application.

### 1. Build the application

For React/Vite applications, generate the production build:

```bash
npm run build
```

This generates the `dist/` directory.

### 2. Configure the application base path

For Vite applications, configure the base path as relative:

```typescript
// vite.config.ts

import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  base: './',
})
```

This ensures that application assets resolve correctly when hosted inside a subdirectory.

### 3. Create an application directory

Create a directory using the application name:

```bash
mkdir -p my-app
```

### 4. Copy the build output

Copy the contents of `dist/` into the application directory:

```bash
cp -a dist/. my-app/
```

The resulting structure should look like:

```text
my-app/
├── assets/
│   ├── index-xxxx.js
│   └── index-xxxx.css
└── index.html
```

**Important:** Copy the contents of `dist/`, not the `dist/` directory itself.

### 5. Commit and push

```bash
git add my-app/
git commit -m "Add my-app"
git push origin main
```

GitHub Actions will automatically deploy all applications.

### 6. Access the application

Once deployment succeeds:

```text
https://<organization>.github.io/tools/my-app/
```

## Deployment

Deployment is managed centrally through:

```text
.github/workflows/deploy-pages.yml
```

The workflow:

1. Checks out the repository.
2. Discovers application directories containing `index.html`.
3. Prepares the static deployment directory.
4. Uploads the Pages artifact.
5. Deploys the artifact to GitHub Pages.

No changes to the workflow are required when adding or removing applications.

You can also trigger deployment manually from:

**GitHub → Actions → Deploy Static Apps to GitHub Pages → Run workflow**

## Application Guidelines

- Each application must have its own top-level directory.
- Each application directory must contain an `index.html` file.
- Include all required assets within the application directory.
- Use relative asset paths (`base: './'` for Vite).
- Applications should be independently buildable.
- Do not commit `node_modules/` or development files.
- Only production build output should be deployed.

## Adding More Applications

Simply create another directory and copy its production build output.

The deployment workflow automatically discovers and publishes it.

No additional GitHub Actions configuration is necessary.
