<script>
	import { base } from '$app/paths';
	import { browser } from '$app/environment';
	import { goto } from '$app/navigation';

	import { onMount } from 'svelte';
	import { Button } from '$lib/components/ui/button';

	let posts;
	let currentPost = { name: '#home' };

	onMount(() => {
		const url = 'https://realskyquest.github.io/posts/posts.json';
		fetchJson(url).then((data) => {
			posts = data.posts;
		});

		let urlParams = new URLSearchParams(window.location.search);
		let urlName = urlParams.get('name');
		let urlLink = urlParams.get('link');

		if (urlName && urlLink) {
			currentPost = { name: urlName, link: urlLink };
		}
	});

	async function fetchJson(url) {
		try {
			const response = await fetch(url);
			if (!response.ok) {
				throw new Error(`HTTP error! status: ${response.status}`);
			}
			const json = await response.json();
			return json;
		} catch (error) {
			console.error('There has been a problem with your fetch operation:', error);
		}
	}

	function removeHTML(a) {
		let b = a.replace('.html', '');
		return b;
	}
</script>

<svelte:head>
	<title>Projects</title>
	<meta name="description" content="This is shows the projects made by realskyquest" />
</svelte:head>

{#if posts}
	<section class="mt-2">
		<div class="flex">
			<div class="w-1/4 border-[#272736] border-2">
				<div class="flex flex-col space-y-4 px-2 mt-2 mb-2" style="overflow-y: auto;">
					<Button
						variant="outline"
						class="border-[#272736]"
						on:click={() => {
							currentPost = { name: '#home' };
							if (browser) {
								goto(`${base}/${currentPost.name}`);
							}
						}}
					>
						Home
					</Button>
					{#each posts as post}
						<Button
							variant="outline"
							class="border-[#272736]"
							on:click={() => {
								currentPost = post;
								if (browser) {
									goto(`${base}/?name=${currentPost.name}&link=${currentPost.link}#post`);
								}
							}}
						>
							{removeHTML(post.name)}
						</Button>
					{/each}
				</div>
			</div>
			<div class="w-3/4 h-screen m-4">
				{#if currentPost.name === '#home'}
					<h2
						id="home"
						class="text-center scroll-m-20 border-b pb-2 text-3xl font-semibold tracking-tight transition-colors first:mt-0"
					>
						Welcome to projects
					</h2>

					<p class="text-center text-xl text-muted-foreground p-10">Latest post</p>

					{#if posts.length > 0}
						<a
							href="{base}/?name={posts[0].name}&link={posts[0].link}#post"
							on:click={() => {
								currentPost = posts[0];
								if (browser) {
									goto(`${base}/?name=${currentPost.name}&link=${currentPost.link}#post`);
								}
							}}
						>
							<div class="h-[180px] pointer-events-none">
								<iframe
									id="post"
									class="w-full h-full rounded-lg"
									title="post"
									src={posts[0].link}
								/>
							</div>
						</a>
					{:else}
						<p class="text-center">No posts found</p>
					{/if}
				{:else}
					<iframe id="post" class="w-full h-full rounded-lg" title="post" src={currentPost.link} />
				{/if}
			</div>
		</div>
	</section>
{/if}
