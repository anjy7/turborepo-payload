# Next.js 13 App Router Payload CMS Monorepo

![Next.js 13 App Router Payload CMS Monorepo](https://raw.githubusercontent.com/infonomic/next-payload-monorepo/main/next-payload-server-og.png)

This is a [Next.js](https://nextjs.org/) [Payload CMS](https://payloadcms.com//), [pnpm](https://pnpm.io/) and [turbo](https://turbo.build/repo) monorepo starter project based on the superb Payload Remix repo here at ... https://github.com/payloadcms/remix-server

## Note

In this configuration the admin panel for Payload CMS will appear under the Next.js application as `/admin` and the Payload api endpoint under `/api`. There are security considerations when hosting both your frontend app and Payload CMS on the same domain and so you should consider whether this setup is approropiate for your application and security needs.

## Features

1. Next.js 13 with App Router and custom Next.js server with Payload CMS integration NOTE: HMR in Next .js 13 is currently broken when using a custom server - see https://github.com/vercel/next.js/issues/50400 https://github.com/vercel/next.js/issues/50461
2. Shared packages for Tailwind CSS, ESLint, and a custom UI kit
3. Configured storybook installation with dark theme switcher for Tailwind CSS
4. A MongoDB example docker-compose.yml configuration to start a fresh MongoDB instance if you need it.

## Getting Started

Clone this repo and then copy

`app/server/.env_example`

to

`app/server/.env`

Generate your Payload CMS secret session key by calling...

`node apps/server/generatePayloadSecret.js`

and then update your `.env` PAYLOAD_SECRET accordingly.

### MongoDB

Optionally start your local MongoDB database (assumes you have Docker or Docker Desktop installed)

```
cd mongodb (you must be in this dir)
mkdir data
./mongo.sh up
```

You can stop and clean up the Docker container and local network...

```
ctrl-C (to stop the container)
./mongo.sh down
```

### Install Dependencies and Run

If you don't already have `pnpm` installed - visit https://pnpm.io/ and follow the instructions there.

In the project root run

`pnpm install`

.. and then

`pnpm dev`

... to start the server and development environment, or

`pnpm dev:seed`

... to start with a seeded Payload CMS including an admin and guest user. Note: Although this repo is configured to run on HOSTNAME=127.0.0.1 - use http://localhost:3000 and http://localhost:3000/admin as opposed to 127.0.0.1 to prevent CORS errors in Payload CMS admin.

To run a production build

`pnpm build` followed by `pnpm serve`

`pnpm clean` will remove all build artifacts and node_modules

You can issues package and app specific commands by using `pnpm` filters - for example...

`pnpm --filter @org/cms outdated`

`pnpm --filter @org/cms update`

## Storybook

To start work on the uikit, change into the packages/uikit directory and run storybook

`cd packages/uikit`

`pnpm storybook`

## Other Issues and Considerations

The Next.js app directory and app router combined with RSC is pretty new tech and I confess I've not spent enough time on any of this to fully understand how RSC works. I understand the basics in terms of new wire format, SSR and client rendering - but there are some very specific requirements for data fetching from within an RSC component and Next.js, and so the [posts](https://github.com/infonomic/next-payload-monorepo/blob/main/apps/web/app/posts/page.tsx) example in this repo is calling the Payload api via fetch (as opposed to the local payload API). This works, and is reasonbly fast as the api endpoint and client app are all hosted on the same server. Take a look at [Data Fetching](https://nextjs.org/docs/app/building-your-application/data-fetching) using the Next.js App router and RSC for more information.

There are almost certainly other issues to consider, for example the current Next.js webpack configuration will not render Radix UI components, even if they are marked as `'use client'`.
