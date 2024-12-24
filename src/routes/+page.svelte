<script lang="ts">
	import { codeToHtml } from 'shiki';
	import { toPng } from 'html-to-image';

	let code = $state('console.log("hi")');
	let lang = $state('typescript');

	const htmlPromise = $derived(
		codeToHtml(code, {
			lang: lang,
			theme: 'github-dark-default',
			colorReplacements: { '#0d1117': 'transparent' }
		})
	);

	const langs = ['js', 'typescript', 'svelte', 'tsx', 'jsx'];
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
	</div>

	<div class="mt-4 max-w-3xl rounded text-2xl">
		{#await htmlPromise then html}
			{@html html}
		{/await}
	</div>
</main>
