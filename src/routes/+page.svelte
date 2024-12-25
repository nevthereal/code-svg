<script lang="ts">
	import { codeToHtml, type BuiltinLanguage } from 'shiki';

	let code = $state('console.log("hi")');
	let lang = $state('typescript');
	let textSize = $state('base');

	const htmlPromise = $derived(
		codeToHtml(code, {
			lang: lang,
			theme: 'github-dark-default'
		})
	);

	const langs: BuiltinLanguage[] = ['js', 'typescript', 'svelte', 'tsx', 'jsx'];
	const sizes = ['xs', 'sm', 'md', 'base', 'lg', 'xl', '2xl', '3xl'];
</script>

<main class="p-6">
	<div class="flex gap-4">
		<textarea
			placeholder="Paste code here :)"
			class="h-[12rem] w-[36rem] resize-none rounded-xl font-mono"
			bind:value={code}
		></textarea>
		<select name="language" id="language-selector" class="mb-auto rounded-xl" bind:value={lang}>
			{#each langs as lang}
				<option value={lang}>{lang}</option>
			{/each}
		</select>
		<select name="language" id="language-selector" class="mb-auto rounded-xl" bind:value={textSize}>
			{#each sizes as size}
				<option value={size}>{size}</option>
			{/each}
		</select>
	</div>

	<div class={`mt-4 rounded-xl bg-[#0D1117] p-4 text-${textSize}`}>
		{#await htmlPromise then html}
			{@html html}
		{/await}
	</div>
</main>
