<script lang="ts">
  import { onMount } from 'svelte'
  import type { AuthSession } from '@supabase/supabase-js'
  import { supabase } from '../../supabaseClient'

  let session: AuthSession | null

  onMount(async () => {
    const { data } = await supabase.auth.getSession()
    session = data.session

    if (!session) {
      window.location.href = '/login'
      return
    }

    supabase.auth.onAuthStateChange((_event, _session) => {
      session = _session
      if (!_session) {
        window.location.href = '/login'
      }
    })
  })
</script>

<div class="min-h-screen bg-gray-50 py-12 px-4 sm:px-6 lg:px-8">
  <div class="max-w-md mx-auto bg-white rounded-lg shadow-md p-8">
    <h1 class="text-2xl font-bold text-gray-900 mb-6">Account Settings</h1>
    
    <div class="space-y-6">
      <div class="text-sm text-gray-600">
        Email: <span class="font-medium">{session?.user?.email}</span>
      </div>
      
      <button
        type="button"
        class="w-full flex justify-center py-2 px-4 border border-gray-300 rounded-md shadow-sm text-sm font-medium text-gray-700 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500"
        on:click={() => supabase.auth.signOut()}
      >
        Sign Out
      </button>
    </div>
  </div>
</div>