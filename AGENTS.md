# AGENTS.md

## Cursor Cloud specific instructions

This is the `@module-federation/vite` plugin — a Vite plugin for Module Federation. It is a **pnpm workspace monorepo** (pnpm 9.1.3, Node 24).

### Key commands

All commands are defined in the root `package.json`:

| Task                | Command                  |
| ------------------- | ------------------------ |
| Install deps        | `pnpm install`           |
| Lint (Prettier)     | `pnpm fmt.check`         |
| Unit tests          | `pnpm test -- --run`     |
| Build library       | `pnpm run build`         |
| Start multi-example | `pnpm run multi-example` |
| E2E tests           | `pnpm run e2e`           |

### Running E2E tests

1. Build the library first: `pnpm run build`
2. Start multi-example servers: `pnpm run multi-example` (background)
3. Wait for servers: `pnpm exec wait-on http://localhost:5173`
4. Run tests: `pnpm run e2e`

Playwright needs Chromium installed: `pnpm exec playwright install --with-deps chromium`

### Multi-example services (ports)

| Service        | Port |
| -------------- | ---- |
| Vite Host      | 5173 |
| Vite Remote    | 4001 |
| Dynamic Remote | 4002 |
| Tests Remote   | 4003 |
| Webpack Remote | 8080 |
| Rspack Remote  | 8081 |

### Gotchas

- The pre-commit hook runs `pretty-quick --staged` via husky. If formatting changes are needed, run `pnpm fmt` (alias for `prettier --write src`) before committing.
- Unit test stderr about `Cannot find module 'dep1/package.json'` is expected — those are test fixtures for missing module resolution.
- `pnpm run build` must complete before `pnpm run multi-example` since examples depend on the built `lib/` output.
- The `microbundle` build emits a warning about `caniuse-lite` being outdated — this is cosmetic and safe to ignore.
