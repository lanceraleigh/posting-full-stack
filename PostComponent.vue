/* eslint-disable */
<template>
  <div class="container">
    <h1>Latest Posts</h1>
    <!-- Create Post Here -->
    <div class="create-post">
      <label for="create-post">Say Something Here...</label>
      <input
        type="text"
        id="create-post"
        v-model="text"
        placeholder="Create a post"
      />
      <button @click="createPost">Post!</button>
    </div>
    <hr />
    <p class="error" v-if="error">{{ error }}</p>
    <div class="posts-container">
      <div
        class="post"
        v-for="(post, index) in posts"
        :item="post"
        :index="index"
        :key="post._id"
      >
        {{
          `${post.createdAt.getDate()}/${
            post.createdAt.getMonth() + 1
          }/${post.createdAt.getFullYear()} at ${post.createdAt.getHours()}:${post.createdAt.getMinutes()}`
        }}
        <p class="text">{{ post.test }}</p>
        <button @click="deletePost(post._id)">Delete Post</button>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */

import PostService from '../PostService';

	export default {
		name: "PostComponent",
		data(){
            return {
                posts: [], 
                error: '',
                text: ''
            }
        },
        async created() {
            try {
                this.posts = await PostService.getPost();
            } catch(err) {
                this.error = err.message;
            }
        },
        methods: {
            async createPost() {
                await PostService.insertPost(this.text);
                this.posts = await PostService.getPost();
            },
            async deletePost(id) {
                await PostService.deletePost(id);
                this.posts = await PostService.getPost();
            }
        }
	};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	h3 {
		margin: 40px 0 0;
	}
	ul {
		list-style-type: none;
		padding: 0;
	}
	li {
		display: inline-block;
		margin: 0 10px;
	}
	a {
		color: #42b983;
	}
</style>
