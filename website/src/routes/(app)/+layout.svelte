<script lang="ts">
	import { AppShell } from '@skeletonlabs/skeleton';
	import Footer from '$lib/Components/Main/Footer.svelte';
	import AppBar from '$lib/Components/Main/AppBar.svelte';
	import { onMount } from 'svelte';
	import { getReleases } from '$lib/utils';
	import { PUBLIC_GH_RELEASES_TK } from '$env/static/public';

	let releases: any = {};
	let is_fetched = false;
	onMount(async () => {
		releases = await getReleases(PUBLIC_GH_RELEASES_TK);
		is_fetched = true;
	});
</script>

{#if is_fetched}
	<AppShell>
		<svelte:fragment slot="header">
			<AppBar version={releases.tag_name} />
		</svelte:fragment>
		<!-- <svelte:fragment slot="sidebarLeft"></svelte:fragment> -->

		<slot />
		<svelte:fragment slot="pageFooter">
			<!-- <Footer version={releases.tag_name} /> -->
		</svelte:fragment>
	</AppShell>
{/if}
