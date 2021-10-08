<script context="module" lang="ts">
	// const localPosts = import.meta.glob('./*.{svx, md}');

	/* * glob imports come from vite
	 * https://vitejs.dev/guide/features.html#glob-import
	 */
	const localPosts = import.meta.glob('../blog/*.{md,svx}');

	let body = [];
	for (let path in localPosts) {
		body.push(
			localPosts[path]().then(({ metadata }) => {
				return { path, metadata };
			})
		);
	}

	export const load = async ({ page }) => {
		const posts = await Promise.all(body);
		const tag = page.params.tag;

		const filteredPosts = posts.filter((post) => {
			return post.metadata.tags.includes(tag);
		});
		console.log(posts, filteredPosts);

		return {
			props: {
				filteredPosts,
				tag
			}
		};
	};
</script>

<script>
	export let filteredPosts;
	export let tag;
</script>

<h1>{tag}</h1>

{#each filteredPosts as { path, metadata: { title } }}
	<li>
		<a href={`${path.replace(/\.\.*\w*/gi, '')}`}>{title}</a>
	</li>
{/each}
