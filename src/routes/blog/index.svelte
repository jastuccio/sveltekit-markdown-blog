<script context="module" lang="ts">
	// const localPosts = import.meta.glob('./*.{svx, md}');

	/* * glob imports come from vite
	 * https://vitejs.dev/guide/features.html#glob-import
	 */
	const localPosts = import.meta.glob('./*.{md,svx}');

	let body = [];
	for (let path in localPosts) {
		body.push(
			localPosts[path]().then(({ metadata }) => {
				return { path, metadata };
			})
		);
	}

	export const load = async () => {
		const posts = await Promise.all(body);

		return {
			props: {
				posts
			}
		};
	};
</script>

<script>
	export let posts;
</script>

<!-- {JSON.stringify(posts)} -->

{#each posts as { path, metadata: { title } }}
	<li>
		<!-- ---------------------------------------------------------
		struggling to get this regex working 
		--------------------------------------------------------- -->
		<a href={`/blog/${path.replace(/\..*/, '')}`}>{title}</a>
	</li>
{/each}
