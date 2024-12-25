<script lang="ts">
	import { codeToHtml, type BuiltinLanguage } from 'shiki';
	import { toPng, toSvg } from 'html-to-image';

	let code = $state(`{#await data.posts}
	<p class="italic font-mono">Loading posts ...</p>
{:then posts}
	<div class="flex flex-col gap-4">
		{#each posts as post}
			<Post {post} currentUserId={userId} />
		{/each}
	</div>
{/await}`);
	let lang = $state('typescript');

	const htmlPromise = $derived(
		codeToHtml(code, {
			lang: lang,
			theme: 'github-dark-default'
		})
	);

	const langs: BuiltinLanguage[] = ['js', 'typescript', 'svelte', 'tsx', 'jsx'];
</script>

<main class="p-6">
	<div class="flex gap-4">
		<textarea
			placeholder="Paste code here :)"
			class="h-[12rem] w-[36rem] resize-none rounded-xl border-2 p-2 font-mono"
			bind:value={code}
		></textarea>
		<div class="flex flex-col gap-2">
			<select
				name="language"
				id="language-selector"
				class="rounded-xl border-2 p-2"
				bind:value={lang}
			>
				{#each langs as lang}
					<option value={lang}>{lang}</option>
				{/each}
			</select>
		</div>
	</div>

	<div class="mt-4 rounded-xl bg-[#0D1117] p-4">
		{#await htmlPromise then html}
			{@html html}
		{/await}
	</div>
</main>
