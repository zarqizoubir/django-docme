<script lang="ts">
	import Navigation from '$lib/Components/Docs/Navigation.svelte';
	import Navbar from '$lib/Components/Docs/Navbar.svelte';
	import { AppShell, Drawer, drawerStore } from '@skeletonlabs/skeleton';
	import { onMount } from 'svelte';
	import NavigationDrawer from '$lib/Components/Docs/NavigationDrawer.svelte';
	import { getReleases } from '$lib/utils';
	import { PUBLIC_GH_RELEASES_TK } from '$env/static/public';

	let releases: any = {};
	$: is_ready = false;
	onMount(async () => {
		releases = await getReleases(PUBLIC_GH_RELEASES_TK);
		is_ready = true;
	});

	const navUrls = [
		{
			title: 'Introduction',
			posts: [
				{ header: 'Introduction', slug: 'introduction', body: 'introduction/introduction.md' },
				{ header: 'Get Started', slug: 'get-started', body: 'introduction/get-started.md' }
			]
		},
		{
			title: 'Api Endpoints',
			posts: [
				{ header: '(GET) /docme', slug: 'get/docme', body: 'api/getdocme.md' },
				{ header: '(POST) /docme', slug: 'post/docme', body: 'api/postdocme.md' },
				{
					header: '(POST) /docme/posts/new',
					slug: 'post/docme/posts/new',
					body: 'api/postdocmepostnew.md'
				},
				{
					header: '(GET) /docme/{slug}',
					slug: 'get/docme/{slug}',
					body: 'api/getdocme{slug}.md'
				},
				{
					header: '(PUT) /docme/{slug}',
					slug: 'put/docme/{slug}',
					body: 'api/putdocme{slug}.md'
				},
				{
					header: '(DELETE) /docme/{slug}',
					slug: 'delete/docme/{slug}',
					body: 'api/deletedocme{slug}.md'
				}
			]
		}
	];
</script>

{#if is_ready}
	<AppShell>
		<svelte:fragment slot="header">
			<Navbar version={releases.tag_name} />
		</svelte:fragment>
		<svelte:fragment slot="sidebarLeft">
			<Navigation {navUrls} />
		</svelte:fragment>
		<slot />
	</AppShell>
{/if}

<Drawer>
	{#if $drawerStore.id === 'nav'}
		<NavigationDrawer {navUrls} />
	{:else}
		<!--  -->
	{/if}
</Drawer>
