<script lang="ts">
  import { supabase } from '../../supabaseClient'
  import type { AuthSession } from '@supabase/supabase-js'

  export let session: AuthSession | null
</script>

<nav class="bg-white shadow">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="flex justify-between h-16">
      <div class="flex">
        <div class="flex-shrink-0 flex items-center">
          <a href="/" class="text-xl font-bold text-blue-600">Your App</a>
        </div>
      </div>

      <div class="flex items-center">
        {#if session}
          <a
            href="/account"
            class="text-gray-700 hover:text-gray-900 px-3 py-2 rounded-md text-sm font-medium"
          >
            Account
          </a>
          <button
            on:click={() => supabase.auth.signOut()}
            class="ml-4 text-gray-700 hover:text-gray-900 px-3 py-2 rounded-md text-sm font-medium"
          >
            Sign Out
          </button>
        {:else}
          <a
            href="/login"
            class="text-gray-700 hover:text-gray-900 px-3 py-2 rounded-md text-sm font-medium"
          >
            Sign In
          </a>
        {/if}
      </div>
    </div>
  </div>
</nav> 