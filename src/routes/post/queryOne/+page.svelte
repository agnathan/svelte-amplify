<script lang="ts">
	import { Amplify } from 'aws-amplify';
	import outputs from '../../../amplify_outputs.json';
	import type { Schema } from '../../../amplify/data/resource';
	import { generateClient } from 'aws-amplify/data';

	Amplify.configure(outputs);
	const client = generateClient<Schema>();

	let postId = '';

	const queryOnePost = async (postId: string) => {
		try {
			const { data: queriedPost } = await client.models.Post.get({
				id: postId
			});
			alert('Post title: ' + queriedPost?.title);
		} catch (error) {
			console.error('Error quering post:', error);
			alert(error);
		}
	};
</script>

<div>
	<label for="postId"><b>postId</b></label>
	<input type="text" placeholder="Enter postId" name="postId" required bind:value={postId} />
	<br /><br />
	<button type="submit" on:click={() => queryOnePost(postId)}>Query one Post</button>
</div>
