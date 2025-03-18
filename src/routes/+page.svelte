<script lang="ts">
  import { onMount } from 'svelte'
  import { supabase } from '../supabaseClient'
  import type { AuthSession } from '@supabase/supabase-js'

  let session: AuthSession | null

  onMount(() => {
    supabase.auth.getSession().then(({ data }) => {
      session = data.session
    })

    supabase.auth.onAuthStateChange((_event, _session) => {
      session = _session
    })
  })
</script>

<div class="min-h-screen bg-gradient-to-b from-gray-50 to-white">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-16">
    <div class="text-center">
      <h1 class="text-4xl tracking-tight font-extrabold text-gray-900 sm:text-5xl md:text-6xl">
        <span class="block">Welcome to</span>
        <span class="block text-blue-600">Your App</span>
      </h1>
      <p class="mt-3 max-w-md mx-auto text-base text-gray-500 sm:text-lg md:mt-5 md:text-xl md:max-w-3xl">
        A modern application built with Svelte and Supabase
      </p>
    </div>

    <div class="mt-10 flex justify-center">
      {#if !session}
        <div class="rounded-md shadow">
          <a
            href="/login"
            class="w-full flex items-center justify-center px-8 py-3 border border-transparent text-base font-medium rounded-md text-white bg-blue-600 hover:bg-blue-700 md:py-4 md:text-lg md:px-10"
          >
            Get Started
          </a>
        </div>
      {:else}
        <div class="rounded-md shadow">
          <a
            href="/account"
            class="w-full flex items-center justify-center px-8 py-3 border border-transparent text-base font-medium rounded-md text-white bg-green-600 hover:bg-green-700 md:py-4 md:text-lg md:px-10"
          >
            Go to Account
          </a>
        </div>
      {/if}
    </div>

    <div class="mt-10">
      <div class="relative">
        <div class="absolute inset-0 flex items-center" aria-hidden="true">
          <div class="w-full border-t border-gray-300"></div>
        </div>
        <div class="relative flex justify-center text-sm">
          <span class="px-2 bg-white text-gray-500">
            {#if session}
              You are currently signed in
            {:else}
              Please sign in to access your account
            {/if}
          </span>
        </div>
      </div>
    </div>
  </div>
</div>