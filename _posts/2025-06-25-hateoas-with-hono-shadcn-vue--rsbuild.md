---
layout: single
title: HATEOAS with Bun, Hono, shadcn-vue, and RSBuild
tags:
  - hateoas
  - hono
  - shadcn
  - tailwind
  - rsbuild
  - bun
  - cli
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: Building backend and frontend architecture with HATEOAS concept.
---
# Why shadcn-vue?

I like alternative and I like stability. I'm no webdev that can waste my time to change the syntax every six months. Vue is stable and shadcn provided the Tailwind-component based with install what you need.

The structure is:
- backend: backend server that serve REST API.
- frontend: that fancy shadcn-vue

## Prerequisites
Before begin, make sure you know that I use `Bun`: A fast JavaScript all-in-one toolkit. 

- [Installation Guide](https://bun.sh/docs/installation)

## Step 1: Create the Project Directory Structure

First, let's create the folder structure for our monorepo.

Open your terminal and create the main project folder:

```bash
mkdir hateoas-boilerplate
cd hateoas-boilerplate
```

Inside hateoas-boilerplate, create the backend and frontend directories:

```bash
mkdir backend frontend
```

## Step 2: Set Up the Backend (Bun + Hono)

Now, let's populate the backend directory.

```bash
bun create hono@latest backend/
```
The options:

```bash
$ bun create hono@latest backend
create-hono version 0.19.1
✔ Using target directory … backend/
✔ Which template do you want to use? bun
✔ Do you want to install project dependencies? Yes
✔ Which package manager do you want to use? bun
✔ Cloning the template
✔ Installing project dependencies
🎉 Copied project files
Get started with: cd backend/
```

The `src/index.ts` -> Use the content from backend/src/index.ts

```ts
import { Hono } from 'hono'

const app = new Hono()

app.get('/', (c) => {
  return c.text('Hello Hono!')
})

export default app
```

## Step 3: Set Up the Frontend (RSBuild + shadcn-vue)

Next, we'll set up the frontend application.

Navigate to the frontend folder (from the root hateoas-boilerplate directory):

```bash
cd ../frontend
```

Create frontend project:

```bash
bun create rsbuild@latest ./
```

Install dependencies:

```bash
bun i zod
bun i -d tailwindcss @tailwindcss/postcss daisyui
```

Populate the files: Copy the content from the corresponding artifacts into each new file.

postcss.config.mjs -> Create new content to frontend/postcss.config.mjs

```js
const config = {
  plugins: {
    '@tailwindcss/postcss': {},
  },
};
export default config;
```

rsbuild.config.ts -> Use content from frontend/rsbuild.config.ts

```ts
import { defineConfig } from '@rsbuild/core';

export default defineConfig({
  source: {
    entry: {
      index: './src/index.ts',
    },
  },
  output: {
    assetPrefix: '/',
  },
  tools: {},
  server: {
    proxy: {
      '/api': {
        target: 'http://localhost:8787',
        changeOrigin: true,
      },
    },
  },
  html: {
    template: './public/index.html',
  },
});
```

public/index.html -> Use content from public/index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HATEOAS Frontend</title>
</head>
<body data-theme="dark">
    <div id="root"></div>
</body>
</html>
```

src/index.css -> Use content from frontend/src/index.css

```css
@import "tailwindcss";
@plugin "daisyui";
```

src/index.ts -> Use content from frontend/src/index.ts

```ts
import { z } from 'zod';
import './index.css';

// Zod schema for the frontend, matching the backend response
const linkSchema = z.object({
  rel: z.string(),
  href: z.string(),
  method: z.string(),
});

const bookSchema = z.object({
  id: z.string(),
  title: z.string(),
  author: z.string(),
  _links: z.array(linkSchema),
});

type Book = z.infer<typeof bookSchema>;

const app = document.getElementById('root');

// --- STATE MANAGEMENT ---
let currentPage = 'books';

// Add a global function to the window object to handle button clicks
declare global {
    interface Window {
        handleAction: (href: string, method: string, bookId?: string) => void;
        navigateTo: (page: string) => void;
    }
}

// --- RENDERING LOGIC ---

function renderApp() {
    if (!app) return;

    const header = `
        <div class="navbar bg-base-100 shadow-md mb-8">
            <div class="flex-1">
                <a class="btn btn-ghost text-xl">HATEOAS Boilerplate</a>
            </div>
            <div class="flex-none">
                <ul class="menu menu-horizontal px-1">
                    <li><a href="#" onclick="window.navigateTo('books')" class="${currentPage === 'books' ? 'active' : ''}">Books</a></li>
                    <li><a href="#" onclick="window.navigateTo('about')" class="${currentPage === 'about' ? 'active' : ''}">About</a></li>
                </ul>
            </div>
        </div>
    `;

    const container = `<div id="page-content" class="container mx-auto p-4 max-w-4xl"></div>`;
    app.innerHTML = header + container;

    const pageContent = document.getElementById('page-content');
    if (currentPage === 'books') {
        renderBooksPage(pageContent);
    } else if (currentPage === 'about') {
        renderAboutPage(pageContent);
    }
}

function renderBooksPage(container: HTMLElement | null) {
    if (!container) return;
    
    const loadingIndicator = `
      <div class="flex justify-center items-center h-64">
        <span class="loading loading-lg"></span>
      </div>
    `;
    container.innerHTML = loadingIndicator;

    fetchBooks(container);
}

function renderAboutPage(container: HTMLElement | null) {
    if (!container) return;

    container.innerHTML = `
        <div class="card bg-base-100 shadow-xl">
            <div class="card-body">
                <h1 class="card-title text-3xl">About This Application</h1>
                <p class="mt-4">This is a boilerplate demonstration of a HATEOAS-driven API and a frontend that consumes it.</p>
                <div class="divider"></div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <h3 class="font-semibold text-lg">Backend</h3>
                        <p>Hono on Bun</p>
                    </div>
                    <div>
                        <h3 class="font-semibold text-lg">Frontend</h3>
                        <p>RSBuild with DaisyUI & TypeScript</p>
                    </div>
                     <div>
                        <h3 class="font-semibold text-lg">Version</h3>
                        <p>1.0.0</p>
                    </div>
                </div>
                 <p class="mt-4 text-sm text-base-content/70">Timestamp of page load: ${new Date().toLocaleString()}</p>
            </div>
        </div>
    `;
}

async function fetchBooks(container: HTMLElement) {
  try {
    const response = await fetch('/api/books');
    if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
    }
    const data = await response.json();
    
    // Validate the response with Zod
    const validatedData = z.object({ _data: z.array(bookSchema) }).parse(data);

    renderBooks(validatedData._data, container);
  } catch (error) {
    console.error("Failed to fetch books:", error);
    if(container) container.innerHTML = `<div class="alert alert-error">Failed to load books. Check the console for details.</div>`;
  }
}

function renderBooks(books: Book[], container: HTMLElement) {
  const bookList = books.map(book => `
    <div class="card w-full bg-base-100 shadow-xl mb-4">
      <div class="card-body">
        <h2 class="card-title">${book.title}</h2>
        <p>By ${book.author}</p>
        <div class="card-actions justify-end mt-4">
          ${book._links.map(link => `
            <button 
              class="btn ${link.method === 'DELETE' ? 'btn-error' : 'btn-primary'}" 
              onclick="window.handleAction('${link.href}', '${link.method}', '${book.id}')"
            >
              ${link.rel}
            </button>
          `).join('')}
        </div>
      </div>
    </div>
  `).join('');

  container.innerHTML = `
    <h1 class="text-4xl font-bold mb-6 text-center">Book Catalog</h1>
    <div>
      ${bookList}
    </div>
  `;
}

// --- NAVIGATION AND ACTIONS ---

window.navigateTo = (page: string) => {
    currentPage = page;
    renderApp();
};

window.handleAction = async (href: string, method: string, bookId?: string) => {
  if (method.toUpperCase() === 'GET') {
      alert(`Navigating to ${href}`);
  } else if (method.toUpperCase() === 'DELETE') {
      if (confirm(`Are you sure you want to delete this item?`)) {
          alert(`Simulating DELETE on ${href}`);
      }
  } else {
    alert(`Action: ${method} on ${href}`);
  }
};

// --- INITIAL LOAD ---
renderApp();

```

## Step 4: Run the Application

Now it's time to bring everything online. You will need two separate terminal windows for this.

### Start the Backend Server:

In your first terminal, navigate to the backend directory.

Run the development server with hot-reloading:

```bash
cd /path/to/hateoas-boilerplate/backend
bun dev
```

You should see a message confirming the server is running, e.g., Server running at http://localhost:8787.

### Start the Frontend Server:

In your second terminal, navigate to the frontend directory.

Run the RSBuild development server:

```bash
cd /path/to/hateoas-boilerplate/frontend
bun dev
```

RSBuild will compile the project and provide you with a URL, typically http://localhost:3000.

## Step 5: Verify Your Setup

Open your web browser and navigate to the URL provided by the frontend server (e.g., http://localhost:3000).

If everything is set up correctly, you should see:

A dark-themed page with the title "Book Catalog".

Two cards, one for "The Hitchhiker's Guide to the Galaxy" and one for "The Lord of the Rings".

Each card will have buttons like self, edit, and delete. Clicking these will trigger a JavaScript alert, demonstrating that the HATEOAS links are being correctly processed by the frontend.

Congratulations! You have successfully set up the fullstack HATEOAS boilerplate.

## Step 5: Making for Production

To make for production, we need to server the HTML compiled in frontend thru the backend. For this, I created a patch script that will create `server.ts` that basically a patched `index.ts` that scans `public` directory.

`scripts/create-server.ts`

```ts
import { readFileSync, writeFileSync, copyFileSync, existsSync } from 'fs'

const inputPath = 'src/index.ts'
const outputPath = 'src/server.ts'

// Check if server.ts already exists and is patched
if (existsSync(outputPath)) {
  const serverContent = readFileSync(outputPath, 'utf-8')
  if (serverContent.includes('serveStatic({ root: "./dist" })')) {
    console.log('✅ server.ts already patched. Skipping.')
    process.exit(0)
  }
}

console.log('📦 Patching server.ts for production...')

// Read original source (dev version)
let content = readFileSync(inputPath, 'utf-8')

// Backup the original (optional)
// copyFileSync(inputPath, `${outputPath}.bak`)

// === STEP 1: Insert static import after `import { cors } ...`
if (!content.includes(`import { cors } from 'hono/cors'`)) {
  console.warn(`⚠️ Could not find CORS import in ${inputPath}. Aborting.`)
  process.exit(1)
}

content = content.replace(
  /import { cors } from 'hono\/cors'/,
  `$&\nimport { serveStatic } from 'hono/bun'`
)

// === STEP 2: Inject static serving BEFORE the CORS middleware
const staticInjection = `app.use('/*', serveStatic({ root: './public' }));\napp.use('/', serveStatic({ path: './public/index.html' }));`
if (!content.includes(`app.use('/api/*', cors({`)) {
  console.warn(`⚠️ Could not find the '/api/*' cors middleware block. Aborting.`)
  process.exit(1)
}

content = content.replace(
  /app\.use\('\/api\/\*', cors\(\{/,
  `${staticInjection}\n$&`
)

// === STEP 3: Replace the full cors(...) block with production version
const corsBlockRegex = /app\.use\('\/api\/\*', cors\(\{[\s\S]*?\}\)\)/

if (!corsBlockRegex.test(content)) {
  console.warn('⚠️ Could not find full CORS middleware block to replace. Aborting.')
  process.exit(1)
}

content = content.replace(
  corsBlockRegex,
  `app.use("/api/*", async (c, next) => { return next() });\napp.use("/*", serveStatic({ root: "./public" }));\napp.notFound(serveStatic({ path: "./public/index.html" }));`
)

// Write final output
writeFileSync(outputPath, content)
console.log('✅ server.ts has been patched and written.')

```


I converted the script from Shell script because Bun script may runs in multiple platforms. This is a boilerplate anyway.

So the new directory structure:
```
hateoas-boilerplate/
├── backend/
│   └── src/
|   └── scripts/
└── frontend/
    ├── public/
    └── src/
```

To make things easier, added little commands on the `package.json`.

### `backend/package.json

```json
{
  "name": "backend",
  "scripts": {
    "dev": "bun run --hot src/index.ts",
    "build:dev": "bun build src/index.ts --compile --outfile=dist/app",
    "build:prod": "bun build src/server.ts --production --target=bun --outfile=dist/app.js",
    "patch:server": "rm -f src/server.ts && bun scripts/create-server.ts",
    "build":"bun patch:server && bun build:prod",
    "clean": "rm -rf dist"
  },
  "dependencies": {
    "hono": "^4.8.1",
    "zod": "^3.25.67"
  },
  "devDependencies": {
    "@types/bun": "latest"
  }
}
```

### `frontend/package.json

```json
{
  "name": "rsbuild-vanilla-ts",
  "version": "1.0.0",
  "private": true,
  "type": "module",
  "scripts": {
    "build": "rsbuild build",
    "check": "biome check --write",
    "dev": "rsbuild dev --open",
    "format": "biome format --write",
    "preview": "rsbuild preview",
    "clean": "rm -rf dist"
  },
  "devDependencies": {
    "@biomejs/biome": "^1.9.4",
    "@rsbuild/core": "^1.3.22",
    "@tailwindcss/postcss": "^4.1.10",
    "daisyui": "^5.0.43",
    "tailwindcss": "^4.1.10",
    "typescript": "^5.8.3"
  },
  "dependencies": {
    "zod": "^3.25.67"
  },
  "trustedDependencies": [
    "@biomejs/biome",
    "@tailwindcss/oxide",
    "core-js"
  ]
}
```

### package.json

```json
{
  "name": "hateoas-app",
  "version": "0.0.1",
  "private": true,
  "type": "module",
  "scripts": {
    "all": "bun --filter './*end'",
    "dev": "bun all dev",
    "dev:backend": "cd backend && bun dev",
    "dev:frontend": "cd frontend && bun dev",
    "build":"bun all build && cp -a backend/dist . && cp -a frontend/dist dist/public",
    "clean":"bun all clean && rm -rf dist"
  }
}
```

## Step 7: Dockerize Your App

Create a Dockerfile in the root of your project. This will be a multi-stage build to keep the final image lean.

```Dockerfile
# ---- Base Stage ----
# Use a specific version for reproducibility
FROM oven/bun:1-alpine AS base
WORKDIR /usr/src/app


FROM base AS frontend-builder

COPY frontend/package.json frontend/bun.lock* .
RUN bun i --frozen-lock
COPY frontend/. .
RUN bun run build

FROM base AS backend-builder

COPY backend/package.json backend/bun.lock* .
RUN bun i --frozen-lock
COPY backend/. .
RUN bun run build

RUN cp $(which bun) bun

# ---- Production Stage ----
# Create the final, lean production image
FROM oven/bun:distroless AS runner

# Set environment variable for production
ENV NODE_ENV=production
WORKDIR /app

# Copy the built frontend static assets from the 'builder' stage
# We'll serve these from a 'public' directory within the backend
COPY --from=frontend-builder /usr/src/app/dist /app/public
COPY --from=backend-builder /usr/src/app/dist/* /app/
# Expose the port the backend server will run on
EXPOSE 8787

# Define the command to run the backend server
CMD ["app.js"]
```

To keep your Docker build context clean and small, create a .dockerignore file in the root directory.

```
# Ignore node_modules for both projects
**/node_modules
**/.DS_Store
**/.vscode

# Ignore build artifacts if they exist locally
frontend/dist

# Ignore local config files that are not needed
.git
.gitignore
README.md
```

Let's test it. 

Build:
```bash
docker build -t hateoas-app .
```

And run it:
```bash
docker run -p 8787:8787 hateoas-app
```
# Acknowledgement

- This article was mostly created using Gemini AI (2.5 Pro) from Google. I use ChatGPT to rewrite my Bash script to Bun script. The end result enhanced by me to include Chainguard image and chaining commands.

- I pick DaisyUI because of [13 best Tailwind CSS component libraries
](https://blog.logrocket.com/13-best-tailwind-css-component-libraries/)

- [Install daisyUI for Rsbuild](https://daisyui.com/docs/install/rsbuild/)
- [Chainguard Container for static](https://images.chainguard.dev/directory/image/static/overview)
- I use filter to run frontend and backend scripts from root so that it won't run in cycle. [Filter](https://bun.sh/docs/cli/filter)
- Just for reminder about [Multi-stage builds](https://docs.docker.com/build/building/multi-stage/)