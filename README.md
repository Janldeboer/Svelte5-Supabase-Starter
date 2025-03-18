# Svelte5 + Supabase Starter

A minimal template for building Svelte5 applications with Supabase integration, including generic pages for `/login` and `/account`.

## Setup

1. Create a project on [Supabase](https://supabase.com)
2. Rename `.env.example` to `.env` and paste in your `SUPABASE_URL` and `SUPABASE_ANON_KEY` from the project
3. Install dependencies:

```bash
npm install
```

## OAuth Setup

The template includes Google OAuth and GitHub OAuth.
Setup instructions to follow.
(Essentially it boils down to creating projects in their respective dev platforms and pasting some values into Supabase)

## Developing

Once you've completed the setup, you can start a development server:

```bash
npm run dev
```

or start the server and open the app in a new browser tab

```
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://svelte.dev/docs/kit/adapters) for your target environment.

## Template Details

This template was created using `npx sv create` with the following options:

- SvelteKit minimal template
- TypeScript with TypeScript syntax
- Add-ons: prettier, eslint, tailwindcss (with typography and forms plugins)
- Package manager: npm
