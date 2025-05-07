<script lang="ts">
	import { Amplify } from 'aws-amplify';
	import outputs from '../../../amplify_outputs.json';
	import type { Schema } from '../../../amplify/data/resource';
	import { generateClient } from 'aws-amplify/data';

	Amplify.configure(outputs);
	const client = generateClient<Schema>();

	let title = '';
	let content = '';

	const addPost = async (title: string, content: string) => {
		try {
			const { data: newPost } = await client.models.Post.create({
				title: title,
				content: content
			});
			alert('Post created successfully! ID: ' + newPost?.id);
		} catch (error) {
			console.error('Error creating post:', error);
			alert('Failed to create post. Please try again.' + error);
		}
	};
</script>

<div>
	<label for="title"><b>Title</b></label>
	<input type="text" placeholder="Enter Title" name="title" required bind:value={title} />
	<br /><br />
	<label for="content"><b>Content</b></label>
	<input type="text" placeholder="Enter Content" name="content" required bind:value={content} />
	<br /><br />
	<button type="submit" on:click={() => addPost(title, content)}>Add Post</button>
</div>
