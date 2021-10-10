<!-- lang="ts" -->
<script context="module">
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
		const localPosts = await Promise.all(body);

		return {
			props: {
				localPosts
			}
		};
	};
</script>

<!-- lang="ts" -->
<script>
	// export let localPosts: [];
	export let localPosts = [];
	console.log('unsorted: ', localPosts);

	const dateSortedPosts = localPosts.sort((post1, post2) => {
		return new Date(post2.metadata.date) - new Date(post1.metadata.date);
	});
	console.log('sorted: ', dateSortedPosts);

	// const postsSortedByDate = localPosts.slice().sort((post1, post2) => {
	// 	return new Date(post2.metadata.date) - new Date(post1.metadate.date);
	// });

	// console.log('sorted', postsSortedByDate);
</script>

<h1>Blog</h1>

{#each localPosts as { path, metadata: { title, tags, date } }}
	<li>
		<a href={`/blog${path.replace(/\.\.*\w*/gi, '')}`}>{title}</a>

		<p>
			{#each tags as tag}
				<a class="tag" href={`/tags/${tag}`}>#{tag}</a>
			{/each}
		</p>
		<p>{new Date(date).toDateString()}</p>
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
