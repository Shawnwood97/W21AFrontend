<template>
  <!-- This seems like a cool way to hide something, rather than all the code I used to do it previously??!?!? -->
  <div v-show="isShowing">
    <h2>{{ postInfo.title }}</h2>
    <p>{{ postInfo.content }}</p>
    <button @click="deletePost">Delete</button>
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
  },
};
</script>

<style lang="scss" scoped></style>
