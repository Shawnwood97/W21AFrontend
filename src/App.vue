<template>
  <div id="app">
    <form action="javascript:void(0)">
      <label for="postTitle">Post Title</label>
      <input type="text" name="postTitle" id="postTitleInput" required />
      <label for="postContent">Post Content</label>
      <textarea type="text" name="postContent" id="postContentInput" required />
      <label for="userId">User Id</label>
      <input type="number" name="userId" id="userIdInput" required />
      <input @click="createPost" type="submit" value="Create Post" />
    </form>
    <div id="postsContainer">
      <ind-post
        class="post"
        v-for="post in posts"
        :key="post.id"
        :postInfo="post"
        :id="`post${post.id}`"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import IndPost from "./components/IndPost.vue";
export default {
  components: { IndPost },
  name: "App",

  data() {
    return {
      posts: undefined,
      postTitle: "",
      postContent: "",
    };
  },

  mounted() {
    axios
      .request({
        url: "http://127.0.0.1:5000/posts",
        method: "GET",
      })
      .then((res) => {
        this.posts = res.data.reverse();
        console.log(res);
      })
      .catch((err) => {
        console.log(err);
      });
  },

  methods: {
    createPost() {
      axios
        .request({
          url: "http://127.0.0.1:5000/posts",
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          data: {
            postTitle: document.getElementById("postTitleInput").value,
            postContent: document.getElementById("postContentInput").value,
            userId: document.getElementById("userIdInput").value,
          },
        })
        .then((res) => {
          console.log(res);
          this.posts.unshift(res.data);
        })
        .catch((err) => {
          console.log(err.response);
        });
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: grid;
  place-items: center;
  gap: 20px;
}

#postsContainer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(600px, 1fr));
  max-width: 90vw;
  gap: 20px;
}

form {
  width: 200px;
  display: grid;
  align-items: center;
}

.post {
  border: 2px solid #000;
  margin-bottom: 5px;
  border-radius: 10px;
  padding: 20px;
  padding-bottom: 10px;
}

button {
  margin-top: 10px;
}
</style>
