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

<div class="container" style="padding: 50px 0 100px 0">
  {#if !session}
    <div class="flex flex-col items-center justify-center">
      <h1 class="text-2xl font-bold mb-4">Welcome to the App</h1>
      <p class="mb-6">Please sign in to access your account.</p>
      <a href="/login" class="bg-blue-500 hover:bg-blue-600 text-white font-medium py-2 px-4 rounded">
        Sign In
      </a>
    </div>
  {:else}
    <div class="flex flex-col items-center justify-center">
      <h1 class="text-2xl font-bold mb-4">Welcome back!</h1>
      <p class="mb-6">You are currently signed in.</p>
      <a href="/account" class="bg-green-500 hover:bg-green-600 text-white font-medium py-2 px-4 rounded">
        Go to Account
      </a>
    </div>
  {/if}
</div>