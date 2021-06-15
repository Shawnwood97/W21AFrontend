<template>
  <!-- This seems like a cool way to hide something, rather than all the code I used to do it previously??!?!? -->
  <div v-show="isShowing">
    <h2>{{ postInfo.title }}</h2>
    <p>{{ postInfo.content }}</p>
    <div class="botContainer">
      <form action="javascript:void(0)">
        <input
          type="text"
          :id="`editTitleInput${postInfo.id}`"
          placeholder="Title"
        />
        <textarea
          type="text"
          :id="`editContentInput${postInfo.id}`"
          placeholder="Content"
        />
        <input @click="editPost" type="submit" value="Edit Post" />
      </form>
      <button @click="deletePost">Delete</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "ind-post",

  props: {
    postInfo: Object,
  },

  data() {
    return {
      isShowing: true,
    };
  },
  methods: {
    deletePost() {
      axios
        .request({
          url: "http://127.0.0.1:5000/posts",
          method: "DELETE",
          headers: {
            "Content-Type": "application/json",
          },
          data: {
            postId: this.postInfo.id,
            userId: this.postInfo.user_id,
          },
        })
        .then((res) => {
          console.log(res);
          this.isShowing = false;
        })
        .catch((err) => {
          console.log(err.response);
        });
    },

    editPost() {
      axios
        .request({
          url: "http://127.0.0.1:5000/posts",
          method: "PATCH",
          headers: {
            "Content-Type": "application/json",
          },
          data: {
            postTitle: document.getElementById(
              `editTitleInput${this.postInfo.id}`
            ).value,
            postContent: document.getElementById(
              `editContentInput${this.postInfo.id}`
            ).value,
            postId: this.postInfo.id,
          },
        })
        .then((res) => {
          console.log(res);
          this.postInfo.title = res.data[0].title;
          this.postInfo.content = res.data[0].content;
        })
        .catch((err) => {
          console.log(err.response);
        });
    },
  },
};
</script>

<style scoped>
.botContainer {
  display: grid;
  grid-auto-flow: column;
}
</style>
