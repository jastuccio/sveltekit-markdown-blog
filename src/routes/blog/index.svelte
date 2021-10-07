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

<h1>Blog</h1>

{#each posts as { path, metadata: { title, tags } }}
	<li>
		<a href={`/blog/${path.replace('.md', '').replace('.svx', '')}`}>{title}</a>

		<p>
			{#each tags as tag}
				<a class="tag" href={`/tags/${tag}`}>#{tag}</a>
			{/each}
		</p>
	</li>
{/each}

<style>
	p {
		margin: 0;
	}
	li {
		margin-bottom: 20px;
	}
	.tag {
		text-decoration: none;
		margin-right: 10px;
		color: #555;
	}
	.tag:hover {
		color: blue;
	}
</style>
