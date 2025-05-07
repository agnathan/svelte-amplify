<script lang="ts">
	import { Amplify } from 'aws-amplify';
	import outputs from '../../../amplify_outputs.json';
	import type { Schema } from '../../../amplify/data/resource';
	import { generateClient } from 'aws-amplify/data';

	Amplify.configure(outputs);
	const client = generateClient<Schema>();

	let postId = '';

	const deletePost = async (postId: string) => {
		try {
			await client.models.Post.delete({
				id: postId
			});
			alert('Post deleted successfully!');
		} catch (error) {
			console.error('Error deleting post:', error);
			alert('Failed to delete post. Please try again.' + error);
		}
	};
</script>

<div>
	<label for="postId"><b>postId</b></label>
	<input type="text" placeholder="Enter postId" name="postId" required bind:value={postId} />
	<br /><br />
	<button type="submit" on:click={() => deletePost(postId)}>Delete Post</button>
</div>
