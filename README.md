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

This template includes OAuth support for Google and GitHub authentication.

### General Setup Flow

Setting up OAuth involves two parts that work together:

1. Configuring your OAuth provider (Google/GitHub)
2. Configuring Supabase to use the provider

I recommend doing these side-by-side to make copy-pasting the credentials and callback URL easier.

### Supabase Setup (Common for All Providers)

1. In your Supabase Dashboard, go to `Authentication -> Sign In / Up`
2. Scroll to `Auth Providers` and select your desired provider
3. Copy the `Callback URL` provided by Supabase (you'll need this for your OAuth provider)
4. Add your provider's credentials (Client ID, Client Secret, etc.)
5. Enable the provider toggle at the top and save

### Provider-Specific Setup

#### Google OAuth

1. Create a project in the [Google Cloud Console](https://console.cloud.google.com/projectcreate)
2. Complete the [Project configuration](https://console.cloud.google.com/auth/overview/create) (select `External` as audience)
3. [Create a new client](https://console.cloud.google.com/auth/clients/create) of type `Web application`
4. Add the Supabase callback URL to `Authorized redirect URIs` in your Google Cloud Console
5. Copy your Google `Client ID` and `Client Secret` to use in Supabase

For more information check [Googles documentation](https://developers.google.com/identity/protocols/oauth2) and the [Supabase _Login with Google_ guide](https://supabase.com/docs/guides/auth/social-login/auth-google)

#### GitHub OAuth

1. Register a new OAuth App in [GitHub Settings -> Developer Settings -> OAuth Apps](https://github.com/settings/applications/new)
2. Insert your `Homepage URL`. You can use `http://localhost:3000/` if you are just not hosting it. Device flow is not needed.
3. Add the Supabase callback URL to the `Authorization callback URL` field
4. After registration, note your `Client ID` and generate a `Client Secret` to use in Supabase

For more information check [GitHubs documentation](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/creating-an-oauth-app) and the [Supabase _Login with GitHub_ guide](https://supabase.com/docs/guides/auth/social-login/auth-github)

#### Other OAuth Providers

The process is similar for other providers Supabase supports:

1. Create an OAuth application with the provider
2. Configure redirect/callback URLs from Supabase
3. Copy credentials to Supabase
4. Enable the provider in Supabase

Check the [Supabase _Social Login_ documentation](https://supabase.com/docs/guides/auth/social-login) for additional information

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
