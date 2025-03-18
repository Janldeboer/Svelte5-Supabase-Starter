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

<Navbar {session} />

{@render children()}
