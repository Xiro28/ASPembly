<script lang="ts">
	import { asset } from '$app/paths';

	let { children } = $props();

	const favicon = asset('/aspembly-minimal-logo.png');
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children()}