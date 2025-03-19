<script lang="ts">
	import '../app.css';
	import { onMount } from 'svelte'
	import { supabase } from '../supabaseClient'
	import type { AuthSession } from '@supabase/supabase-js'
	import Navbar from '$lib/components/Navbar.svelte'

	let { children } = $props();
	let session = $state<AuthSession | null>(null)

	onMount(() => {
		supabase.auth.getSession().then(({ data }) => {
			session = data.session
		})

		supabase.auth.onAuthStateChange((_event, _session) => {
			session = _session
		})
	})
</script>

<div class="flex flex-col min-h-screen bg-gray-50">
	<Navbar {session} />
	<main class="flex-1 flex flex-col">
		{@render children()}
	</main>
</div>
