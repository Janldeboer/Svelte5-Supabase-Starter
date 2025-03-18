<script lang="ts">
  import { onMount } from 'svelte'
  import type { AuthSession } from '@supabase/supabase-js'
  import { supabase } from '../../supabaseClient'

  let session: AuthSession | null

  let loading = false
  let username: string | null = null
  let website: string | null = null
  let avatarUrl: string | null = null

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

    await getProfile()
  })

  const getProfile = async () => {
    try {
      loading = true
      console.log('getProfile')
      console.log('session', session)
      if (!session) throw new Error('Session not found')
      const { user } = session

      const { data, error, status } = await supabase
        .from('profiles')
        .select('username, website, avatar_url')
        .eq('id', user.id)
        .single()

      if (error && status !== 406) throw error

      if (data) {
        username = data.username
        website = data.website
        avatarUrl = data.avatar_url
      }
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      loading = false
    }
  }

  const updateProfile = async () => {
    try {
      loading = true
      const { user } = session

      const updates = {
        id: user.id,
        username,
        website,
        avatar_url: avatarUrl,
        updated_at: new Date().toISOString(),
      }

      const { error } = await supabase.from('profiles').upsert(updates)

      if (error) {
        throw error
      }
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      loading = false
    }
  }
</script>

<div class="min-h-screen bg-gray-50 py-12 px-4 sm:px-6 lg:px-8">
  <div class="max-w-md mx-auto bg-white rounded-lg shadow-md p-8">
    <h1 class="text-2xl font-bold text-gray-900 mb-6">Account Settings</h1>
    
    <form on:submit|preventDefault="{updateProfile}" class="space-y-6">
      <div class="text-sm text-gray-600">
        Email: <span class="font-medium">{session?.user?.email}</span>
      </div>
      
      <div>
        <label for="username" class="block text-sm font-medium text-gray-700">Name</label>
        <input
          id="username"
          type="text"
          bind:value="{username}"
          class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500 sm:text-sm"
        />
      </div>
      
      <div>
        <label for="website" class="block text-sm font-medium text-gray-700">Website</label>
        <input
          id="website"
          type="text"
          bind:value="{website}"
          class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500 sm:text-sm"
        />
      </div>
      
      <div class="space-y-4">
        <button
          type="submit"
          class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 disabled:opacity-50 disabled:cursor-not-allowed"
          disabled="{loading}"
        >
          {loading ? 'Saving...' : 'Update profile'}
        </button>
        
        <button
          type="button"
          class="w-full flex justify-center py-2 px-4 border border-gray-300 rounded-md shadow-sm text-sm font-medium text-gray-700 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500"
          on:click={() => supabase.auth.signOut()}
        >
          Sign Out
        </button>
      </div>
    </form>
  </div>
</div>