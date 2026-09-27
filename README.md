# Preflight - Database

## Setup

- Use branch `full-database` (not `pf-batabase`)
- `pnpm install`
- Create `.env` from `.env.example`
- DB container
  - `pnpm run eol` to set the correct line endings (Windows)
  - `chmod +x ./_entrypoint/*.sh` to make scripts executable (Linux/Mac)
  - `docker compose up -d` to start the database

## Usage

- `pnpm run db:migrate` to apply migrations in `db/migration`
- `pnpm run db:prototype` to seed test data (deletes all existing data; change `patiphan`'s `cmuAccount` in `db/prototype.ts` to your CMU account first)
- `pnpm run db:generate` after editing `db/schema.ts` (run in a real terminal)
- `pnpm run db:push`

## Two-tsconfig Setup

Right now the `drizzle.config.ts` file will be included in the `dist` folder when building the app, which is not ideal. To fix this, we have two `tsconfig` files:

- `tsconfig.json (type-check / IDE)`

```
{
  "extends": [
    "@tsconfig/node-lts/tsconfig.json",
    "@tsconfig/node-ts/tsconfig.json"
  ],
  "compilerOptions": {
    "types": ["node"],
    "rootDir": ".",
    "outDir": "./dist",
    "paths": {
      "@db/*": ["./db/*"]
    }
  },
  "include": ["./db/**/*", "./drizzle.config.ts"]
}
```

- `tsconfig.build.json (emit only app code)`

```
{
  "extends": "./tsconfig.json",
  "exclude": ["./drizzle.config.ts"]
}
```
