---
layout: single
title: Setup Gowebly  
tags:
  - degit
  - bun
  - cli
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: Install `degit`
---
# HATEOAS Boilerplate Setup Guide

This tutorial will walk you through setting up the complete project, including the directory structure, installing dependencies, and running both the backend and frontend servers.

## Prerequisites
Before you begin, make sure you have the following installed on your system:

- `Bun`: A fast JavaScript all-in-one toolkit. [Installation Guide](https://bun.sh/docs/installation)

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

Set up the nested directories for both applications:

```bash
# For the backend
mkdir -p backend/src

# For the frontend
mkdir -p frontend/src frontend/public
```

After this step, your project structure should look like this:
```
hateoas-boilerplate/
├── backend/
│   └── src/
└── frontend/
    ├── public/
    └── src/
```

## Step 2: Set Up the Backend (Bun + Hono)

Now, let's populate the backend directory.

Navigate to the backend folder:

```bash
cd backend
```

Create the backend project:

```bash
bun create hono@latest ./
```

Install dependencies:

```bash
bun i zod
```


Populate the files: Copy the content from the corresponding artifacts into the files you just created.

- src/index.ts -> Use the content from backend/src/index.ts
```ts
import { Hono } from 'hono'
import { validator } from 'hono/validator'
import { z } from 'zod'
import { cors } from 'hono/cors'

const app = new Hono()

// Add CORS middleware to allow requests from the frontend
app.use('/api/*', cors({
  origin: 'http://localhost:3000', // RSBuild default dev server port
  allowMethods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS'],
  allowHeaders: ['Content-Type']
}))

// Define a schema for our resource
const bookSchema = z.object({
  id: z.string(),
  title: z.string(),
  author: z.string(),
});

type Book = z.infer<typeof bookSchema>;

// Mock database
let books: Book[] = [
  { id: '1', title: 'The Hitchhiker\'s Guide to the Galaxy', author: 'Douglas Adams' },
  { id: '2', title: 'The Lord of the Rings', author: 'J.R.R. Tolkien' },
];

// HATEOAS link generator
const generateBookLinks = (book: Book) => {
  return [
    { rel: 'self', href: `/api/books/${book.id}`, method: 'GET' },
    { rel: 'edit', href: `/api/books/${book.id}`, method: 'PUT' },
    { rel: 'delete', href: `/api/books/${book.id}`, method: 'DELETE' },
  ]
}

// Collection endpoint
app.get('/api/books', (c) => {
  const booksWithLinks = books.map(book => ({
    ...book,
    _links: generateBookLinks(book)
  }));
  return c.json({
    _data: booksWithLinks,
    _links: {
      self: { href: '/api/books', method: 'GET' },
      create: { href: '/api/books', method: 'POST' },
    }
  })
})

// Single resource endpoint
app.get('/api/books/:id', (c) => {
  const book = books.find(b => b.id === c.req.param('id'))
  if (!book) {
    return c.json({ error: 'Book not found' }, 404)
  }
  return c.json({
    ...book,
    _links: generateBookLinks(book)
  })
})

// Create resource endpoint
const createBookSchema = bookSchema.omit({ id: true });

app.post(
  '/api/books',
  validator('json', (value, c) => {
    const parsed = createBookSchema.safeParse(value);
    if (!parsed.success) {
      return c.json({ error: 'Invalid input', issues: parsed.error.issues }, 400)
    }
    return parsed.data;
  }),
  (c) => {
    const newBookData = c.req.valid('json');
    const newBook: Book = {
      id: (books.length + 1).toString(),
      ...newBookData
    }
    books.push(newBook);
    return c.json(newBook, 201)
  }
)

console.log("Server running at http://localhost:8787")

export default {
  port: 8787,
  fetch: app.fetch,
}

```

## Step 3: Set Up the Frontend (RSBuild + DaisyUI)

Next, we'll set up the frontend application.

Navigate to the frontend folder (from the root hateoas-boilerplate directory):

```bash
cd ../frontend
```

Create frontend project:

```bash
bun i zod
bun create rsbuild@latest ./
```

Install dependencies:

```bash
bun i zod
bun i -d tailwindcss @tailwindcss/postcss daisyui
```

Note: Don't forget the tsconfig.json for the frontend as well.

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

// Add a global function to the window object to handle button clicks
// This is a simple way to handle events on dynamically generated HTML
declare global {
    interface Window {
        handleAction: (href: string, method: string, bookId?: string) => void;
    }
}


async function fetchBooks() {
  const loadingIndicator = `
    <div class="flex justify-center items-center h-screen">
      <span class="loading loading-lg"></span>
    </div>
  `;
  if(app) app.innerHTML = loadingIndicator;

  try {
    const response = await fetch('/api/books');
    if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
    }
    const data = await response.json();
    
    // Validate the response with Zod
    const validatedData = z.object({ _data: z.array(bookSchema) }).parse(data);

    renderBooks(validatedData._data);
  } catch (error) {
    console.error("Failed to fetch books:", error);
    if(app) app.innerHTML = `<div class="alert alert-error">Failed to load books. Check the console for details.</div>`;
  }
}

function renderBooks(books: Book[]) {
  if (!app) return;

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

  app.innerHTML = `
    <div class="container mx-auto p-4 max-w-4xl">
      <h1 class="text-4xl font-bold mb-6 text-center">Book Catalog</h1>
      <div>
        ${bookList}
      </div>
    </div>
  `;
}

// Handler for HATEOAS actions
window.handleAction = async (href: string, method: string, bookId?: string) => {
  if (method.toUpperCase() === 'GET') {
      alert(`Navigating to ${href}`);
      // In a real app, you would fetch the single item and display it
  } else if (method.toUpperCase() === 'DELETE') {
      if (confirm(`Are you sure you want to delete this item?`)) {
          alert(`Simulating DELETE on ${href}`);
          // In a real app:
          // await fetch(href, { method: 'DELETE' });
          // fetchBooks(); // Refresh the list
      }
  } else {
    alert(`Action: ${method} on ${href}`);
  }
};

// Initial fetch
fetchBooks();
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