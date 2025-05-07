<script lang="ts">
	import { Amplify } from 'aws-amplify';
	import outputs from '../../../amplify_outputs.json';
	import type { Schema } from '../../../amplify/data/resource';
	import { generateClient } from 'aws-amplify/data';
	import { onMount } from 'svelte';

	Amplify.configure(outputs);
	const client = generateClient<Schema>();

	let posts: Schema['Post']['type'][] = [];

	const queryAllPosts = async () => {
		try {
			const { data: items } = await client.models.Post.list();
			posts = items;
		} catch (error) {
			console.error('Error quering all posts:', error);
			alert(error);
		}
	};

	onMount(() => {
		queryAllPosts();
	});
</script>

{#if posts.length > 0}
	<table>
		<thead>
			<tr>
				<th>ID</th>
				<th>Title</th>
				<th>Content</th>
			</tr>
		</thead>
		<tbody>
			{#each posts as post (post.id)}
				<tr>
					<td>{post.id}</td>
					<td>{post.title}</td>
					<td>{post.content}</td>
				</tr>
			{/each}
		</tbody>
	</table>
{:else}
	<p>No posts found.</p>
{/if}
