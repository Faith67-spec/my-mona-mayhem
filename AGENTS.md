# Mona Mayhem — Copilot Agent Guide

**Project**: GitHub Contribution Battle Arena built with Astro  
**Tech Stack**: Astro 6.4.6 + Node.js adapter + Astro Island architecture

## Project Overview

Mona Mayhem is a retro arcade-themed web app that compares GitHub contribution graphs between two users. The app fetches contribution data via the GitHub API and displays it in a browser-based battle arena interface.

**Key Features**:
- Compare two GitHub users' contribution activity
- Retro gaming visual design (Press Start 2P font)
- Server-side rendered with Node.js adapter
- API endpoint for fetching user contribution data

## Quick Start Commands

```bash
# Development (with hot reload)
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Run specific Astro commands
npm run astro -- [command]
```

**Development Server**: Runs at `http://localhost:4321/` by default

## Project Structure

```
src/
├── pages/
│   ├── index.astro          # Main landing page
│   └── api/
│       └── contributions/
│           └── [username].ts # Dynamic API route for fetching user data
public/                        # Static assets (favicon, etc.)
docs/                         # Localized documentation
workshop/                     # Lesson guides (ignore for code)
```

## Astro Best Practices for This Project

### 1. **File-Based Routing**
- Pages in `src/pages/` automatically become routes
- Dynamic segments use brackets: `[username].ts` creates routes like `/api/contributions/octocat`
- Use `getStaticPaths()` for pre-rendering or `output: 'server'` for on-demand rendering

### 2. **Astro Components (`.astro` files)**
- Mix HTML, CSS, and JavaScript in single `.astro` files
- Use the frontmatter (between `---` delimiters) for server-side logic
- Components are **zero-JS by default** — they only send HTML to the browser
- Use `client:*` directives to make components interactive: `client:load`, `client:idle`, `client:visible`

### 3. **API Routes**
- Files in `src/pages/api/` are treated as endpoints
- TypeScript API routes: export `GET`, `POST`, `PUT`, `DELETE` functions
- Example from this project: `src/pages/api/contributions/[username].ts`
  ```ts
  export async function GET({ params }) {
    const { username } = params;
    // Fetch from GitHub API or return JSON data
    return new Response(JSON.stringify(data));
  }
  ```

### 4. **Server vs. Client Rendering**
- This project uses `output: 'server'` with Node.js adapter → on-demand server rendering
- No pre-build step needed; routes render when requested
- For dynamic routes like `/api/contributions/[username]`, responses are computed on each request

### 5. **Styling**
- CSS is scoped to components by default in `.astro` files
- Global styles go in `src/styles.css` and can be imported into pages
- Use CSS modules (`.module.css`) for component-specific scoping

### 6. **Environment & Configuration**
- Config in `astro.config.mjs`
- Environment variables in `.env` files (loaded automatically by Astro)
- Current adapter: `@astrojs/node` with `mode: 'standalone'` for production builds

## Common Development Patterns

### Fetching Data in Page Frontmatter
```astro
---
// This runs ONLY on the server
const user = await fetch(`https://api.github.com/users/octocat`).then(r => r.json());
---
<h1>Welcome, {user.name}</h1>
```

### Dynamic API Routes
```ts
// src/pages/api/contributions/[username].ts
export async function GET({ params }) {
  const { username } = params;
  try {
    const data = await fetchContributions(username);
    return new Response(JSON.stringify(data), {
      headers: { 'Content-Type': 'application/json' }
    });
  } catch (error) {
    return new Response(JSON.stringify({ error: 'Not found' }), { status: 404 });
  }
}
```

### Using Islands for Interactivity
```astro
---
import InteractiveComponent from '../components/Game.astro';
---
<InteractiveComponent client:load />
```

## Debugging Tips

- **Hot reload issues**: Restart `npm run dev` if changes aren't reflecting
- **API errors**: Check Network tab in browser DevTools for request/response details
- **Build failures**: Run `npm run build` to catch production-only errors
- **Type errors**: Run `astro check` to validate TypeScript (if configured)

## Notes

- Workshop materials are in `workshop/` — not part of the app codebase
- The project currently has 11 non-blocking npm vulnerabilities in transitive dependencies; these don't prevent development
- Localized docs (pt_BR, es) are in `docs/` — sync changes across all language versions if updating docs

---

**For more Astro docs**: https://docs.astro.build/
