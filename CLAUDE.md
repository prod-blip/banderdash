# CLAUDE.md — interactive essays blog

A personal blog of interactive essays: prose-first writing that breaks, at chosen moments, into interactive or gamified set-pieces. Reading is the base experience; any interaction must *enact* the meaning, never just decorate it.

## Architecture

- **Astro** — content-first; ships zero JS by default, so only interactive pieces hydrate (islands).
- **MDX** — posts are `.mdx` files: Markdown prose with components dropped inline.
- **Svelte** — interactive islands (animation, game state) via `@astrojs/svelte`. Purely static pieces can be `.astro`.
- **TypeScript** throughout. Plain CSS with light/dark design tokens.
- **Static deploy** to Vercel or Cloudflare Pages.
- Add animation, canvas, or persistence libraries only when a specific piece actually needs them.

## How we work

Discuss first, build second. Do **not** implement anything — especially interactive elements — until it has been talked through with the author and explicitly signed off. Site plumbing (scaffold, layout, routing, typography, deploy) can proceed normally; all creative and interactive design decisions are made together, in conversation, before any code is written.
