# Copilot Instructions for Mona Mayhem

**Project**: GitHub Contribution Battle Arena — a retro arcade-themed web app built with Astro  
**Tech Stack**: Astro 6.4.6 + Node.js adapter + TypeScript  
**Quick Orientation**: See [AGENTS.md](../AGENTS.md) for comprehensive project guide, Astro best practices, and common development patterns.

## Essential Commands

```bash
npm run dev      # Development server (http://localhost:4321/)
npm run build    # Production build
npm run preview  # Preview production build
npm run astro -- [cmd]  # Run raw Astro CLI commands
```

## Key Architecture

- **File-based routing**: Pages in `src/pages/` → routes. Dynamic segments use brackets: `[username].ts`
- **Server-rendered**: `output: 'server'` with `@astrojs/node` — routes render on-demand, no pre-build
- **API Routes**: Files in `src/pages/api/` export `GET`, `POST`, etc. functions
- **Zero-JS by default**: `.astro` components send HTML only. Add interactivity with `client:load` directive
- **Scoped CSS**: Styling in `.astro` files is automatically scoped to that component

## Current Implementation Status

| File | Status |
|------|--------|
| `src/pages/index.astro` | Basic starter template |
| `src/pages/api/contributions/[username].ts` | Ready for GitHub API integration (currently returns 501) |

## Important Notes

- **Workshop materials** (`workshop/`) are lesson guides — ignore for code work
- **Localized docs** (`docs/es/`, `docs/pt_BR/`) should be kept in sync if updating docs
- **Dependencies**: Minor npm vulnerabilities in transitive deps (non-blocking for development)

## Development Patterns

### Fetch data in page frontmatter (server-side only)
```astro
---
const data = await fetch('https://api.github.com/users/octocat').then(r => r.json());
---
<h1>{data.name}</h1>
```

### API route example
```ts
export const GET: APIRoute = async ({ params }) => {
  const { username } = params;
  // Fetch data, return JSON
  return new Response(JSON.stringify(data), {
    headers: { 'Content-Type': 'application/json' }
  });
};
```

For more details, see [AGENTS.md](../AGENTS.md).
