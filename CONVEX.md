# Convex Setup (Product Suite)

This doc covers how to work with Convex for the **product-suite** project.

## Prerequisites
- Bun installed
- Convex CLI (use `bunx convex` without a global install)
- Access to the Convex project: **product-suite**

## Project Details (Created)
- **Project:** product-suite
- **Team:** nintu
- **Dashboard:** https://dashboard.convex.dev/t/nintu/product-suite
- **Dev Deployment:** dev:admired-starfish-836
- **Public URL:** https://admired-starfish-836.convex.cloud

## Environment Variables
Add these to `.env.local` or `.env` (depends on framework):

```
CONVEX_DEPLOYMENT=dev:admired-starfish-836
NEXT_PUBLIC_CONVEX_URL=https://admired-starfish-836.convex.cloud
```

`CONVEX_DEPLOYMENT` and `NEXT_PUBLIC_CONVEX_URL` are auto‑written by `convex dev`.

## Initialize Convex (if needed)
If the `convex/` directory doesn’t exist, run:

```
bunx convex dev --once
```

This will:
- Create the `convex/` folder
- Provision a dev deployment
- Populate `.env.local`

## Typical Commands
Login to Convex (once per machine):

```
bunx convex login
```

Start Convex dev:

```
bunx convex dev
```

Deploy to production (once configured):

```
bunx convex deploy --prod
```

## Notes
- `convex/` contains your server functions, schema, and configuration.
- See `convex/README.md` for default Convex project structure.
