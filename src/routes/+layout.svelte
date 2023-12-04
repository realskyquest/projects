<script>
	import '../app.pcss';

	import { base } from '$app/paths';

	import { Home, FolderKanban, Lightbulb, LightbulbOff, Copyright } from 'lucide-svelte';
	import { ModeWatcher, toggleMode } from 'mode-watcher';

	import { Button } from '$lib/components/ui/button';
	import Avatar from '$lib/components/Avatar.svelte';
</script>

<ModeWatcher />

<header>
	<div class="w-full mt-20 mb-6">
		<Avatar></Avatar>
		<h1 class="text-center mt-4 scroll-m-20 text-4xl font-extrabold tracking-tight lg:text-5xl">
			realSkyQuest
		</h1>
	</div>
</header>

<nav class="justify-between flex p-8 bg-[#43434f] text-white">
	<ul class="flex space-x-4 mx-auto">
		<li class="mt-[0.4rem]">
			<a href="https://realskyquest.github.io/" class="hover:underline flex space-x-1">
				<Home />
				<p>Home</p>
			</a>
		</li>
		<li class="mt-[0.4rem]">
			<a href="https://realskyquest.github.io/projects" class="hover:underline flex space-x-1">
				<FolderKanban />
				<p>Projects</p>
			</a>
		</li>
		<li>
			<Button on:click={toggleMode} variant="secondary" size="icon" class="mx-auto">
				<Lightbulb
					class="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0"
				/>
				<LightbulbOff
					class="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100"
				/>
				<span class="sr-only">Toggle theme</span>
			</Button>
		</li>
	</ul>
</nav>

<slot />
