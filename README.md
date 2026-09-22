# OutSpot — Backend

REST and realtime API for [OutSpot](https://ayan-parvaiz.web.app/work/outspot), a
location-based social platform. Node.js and Express over PostgreSQL, serving the
Flutter clients on iOS, Android, Web, macOS and Windows.

## Stack

| Layer | Choice |
|---|---|
| Runtime | Node.js, Express |
| Database | PostgreSQL via Prisma |
| Realtime | Socket.IO |
| Media | AWS S3 (`multer-s3`), Sharp for resizing |
| Push | Firebase Admin — FCM and APNs |
| Scheduling | `node-cron` |
| Mail | Nodemailer |
| AI | OpenAI |
| Admin UI | EJS server-rendered panel |

## What it covers

Fourteen public route modules and a separate admin surface:

**Public** — auth, chat, community feeds, challenges, explore, friends,
leaderboard, map, media upload, notifications, referrals, reporting, shop.

**Admin** — dashboard, user and content moderation, challenge authoring,
locations, points, premade content, reports, shop management, body shapes.

Auth is JWT-based, split across `authMiddleware`, `adminAuth` and
`requireAdmin` so the admin surface is gated independently of user sessions.

A midnight scheduler (`schedulers/midnightChallengeScheduler.js`) rotates daily
challenges and fans out the accompanying notifications.

## Running locally

```bash
npm install
npx prisma generate
npx prisma migrate deploy
node server.js
```

Configuration is read from a `.env` file, which is deliberately not committed.
It needs at minimum a `DATABASE_URL`, the AWS S3 credentials and bucket, the
Firebase service-account values, the SMTP settings and an OpenAI key.

## Repository layout

```
controllers/   route handlers
routes/        public API routes
routes/admin/  admin panel routes
middlewares/   auth and role guards
prisma/        schema and migrations
schedulers/    cron jobs
scripts/       one-off maintenance and seed scripts
utils/         shared helpers
public/        static assets for the admin panel
```

## Deployment

Runs on AWS EC2 behind the usual Node process manager. Connection details and
keys are held outside this repository — ask the maintainer if you need access.
