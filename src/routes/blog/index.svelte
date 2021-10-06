<script context="module">
	// const localPosts = import.meta.glob('./*.{svx, md}');
	const localPosts = import.meta.glob('./*.*');

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
		<a href={`/blog/${path.replace('.md', '').replace('.svx', '')}`}>{title}</a>

		<!-- <a href={`/blog/${path.replace(/\..*/g, '')}`}>{title}</a> -->
	</li>
{/each}
