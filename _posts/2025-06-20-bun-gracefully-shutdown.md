---
layout: single
title: Gracefully Shutdown in Bun
tags:
  - hono
  - bun
  - docker
  - cli
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: Make it stop in Docker.
---
I used to think NodeJS nor its alternatives never have any clue for listening to OS quit signals. I used to put `tini` to make it stopped.

Actually, we can stop them. It's just the code must be added manually. The code:

```ts
process.on('SIGINT', () => {
  console.log('Received SIGINT. Performing graceful shutdown...');
  // Perform cleanup operations here, e.g., close database connections,
  // close open files, stop accepting new requests, etc.
  process.exit(0); // Exit after cleanup
});

process.on('SIGTERM', () => {
  console.log('Received SIGTERM. Performing graceful shutdown...');
  // Perform cleanup operations here
  process.exit(0); // Exit after cleanup
});
```

To make us in the same page:
```bash
bun create hono@latest sample-app
```

We modify the `src/index.ts` so it would be like this:

```ts
import { Hono } from 'hono'

const app = new Hono()

app.get('/', (c) => {
  return c.text('Hello Hono!')
})

process.on('SIGINT', () => {
  console.log('Received SIGINT. Performing graceful shutdown...');
  // Perform cleanup operations here, e.g., close database connections,
  // close open files, stop accepting new requests, etc.
  process.exit(0); // Exit after cleanup
});

process.on('SIGTERM', () => {
  console.log('Received SIGTERM. Performing graceful shutdown...');
  // Perform cleanup operations here
  process.exit(0); // Exit after cleanup
});

export default app
```

Now our app will listen to the sound of her people.
