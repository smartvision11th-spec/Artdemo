# ArtSell Vercel Rebuild

This is the Vercel-independent rebuild of the supplied ArtSell application.

## Stack

- Next.js App Router
- React
- PostgreSQL
- Prisma ORM
- Vercel Blob for artwork images
- Stripe Checkout
- Zustand for the guest cart
- TanStack Query for client data fetching
- Tailwind CSS
- No Caffeine, ICP, Motoko, Internet Identity, Caffeine object storage or Caffeine runtime dependencies

## Local setup

1. Create a PostgreSQL database.
2. Copy `.env.example` to `.env`.
3. Fill in:
   - `DATABASE_URL`
   - `BLOB_READ_WRITE_TOKEN`
   - `STRIPE_SECRET_KEY`
   - `STRIPE_WEBHOOK_SECRET`
   - `ADMIN_PASSWORD`
   - `ADMIN_SESSION_SECRET`
   - `NEXT_PUBLIC_SITE_URL`
4. Install dependencies:

```bash
npm install
```

5. Create the database schema:

```bash
npx prisma db push
```

6. Optional demo data:

```bash
npm run db:seed
```

7. Start:

```bash
npm run dev
```

## Stripe

Create a Stripe webhook pointing to:

`https://YOUR-DOMAIN.com/api/stripe/webhook`

Subscribe to `checkout.session.completed`.

The checkout route uses the server-side PostgreSQL prices, so browser-side price manipulation is not trusted.

## Vercel

Import the GitHub repository into Vercel.

Add the same environment variables in Vercel Project Settings → Environment Variables.

The build command is:

```bash
npm run build
```

The build automatically runs `prisma generate`.

After the first deployment, apply the production database schema from a trusted environment:

```bash
npx prisma db push
```

For a mature production workflow, use versioned Prisma migrations instead.

## Images

Artwork uploads are handled by `/api/admin/artworks` and `/api/admin/artworks/[id]`.

They use Vercel Blob, so images are not stored inside the Vercel deployment filesystem.

## Admin

Open `/admin` and enter the value configured in `ADMIN_PASSWORD`.

Do not put the admin password in client-side code.

## Data migration

The uploaded source did not include the live Caffeine canister contents or its stored image blobs. See `MIGRATION_PLAN.md` for the exact mapping and migration limitation.
