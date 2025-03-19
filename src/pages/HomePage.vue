<script setup>
import { computed, ref } from "vue";
import PostCard from "@/components/PostCard.vue";
const text = ref("");

const trimmedText = computed(() => text.value.trim());

const posts = ref([]);
// const sortedPost = computed(() =>
//   posts.value.toSorted((postA, postB) => postB.createdAt - postA.createdAt)
// );

function addPost() {
  // const newPost = {
  //   id: Math.random().toString(36).substring(2),
  //   content: trimmedText.value,
  //   createdAt: new Date(),
  //   author: {
  //     username: "Nono",
  //     avatarUrl:
  //       "https://media.licdn.com/dms/image/v2/C4D03AQH0TuKfHrOpfQ/profile-displayphoto-shrink_200_200/profile-displayphoto-shrink_200_200/0/1607679351861?e=2147483647&v=beta&t=DQp5RzUGjvkvOkY3dF-EieiqoIV6RVdKdJrpnvaOrpM",
  //   },
  // };
  // posts.value.push(newPost);
  // text.value = "";

  const token = JSON.parse(localStorage.getItem("user")).token;
  fetch("https://posts-crud-api.vercel.app/posts", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer ${token}`,
    },
    body: JSON.stringify({
      content: trimmedText.value,
    }),
  })
    .then((response) => response.json())
    .then((data) => {
      console.log(data);
      text.value = "";
    });
}

function deletePost(postId) {
  posts.value = posts.value.filter((post) => post.id !== postId);
}

function likePost(PostId) {
  for (let i = 0; i < posts.value.length; i++) {
    if (posts.value.at(i).id == PostId) {
      posts.value.at(i).liked = !posts.value.at(i).liked;
      break;
    }
  }
}
const loading = ref(false);
const apiPosts = ref([]);
function fetchPost() {
  loading.value = true;
  const result = fetch("https://posts-crud-api.vercel.app/posts");
  result
    .then((response) => response.json())
    .then((data) => {
      apiPosts.value = data;
      loading.value = false;
    });
}

fetchPost();
</script>

<template>
  <main>
    <div class="container">
      <form class="card" @submit.prevent="addPost">
        <textarea
          name="post"
          id="post"
          placeholder="Quoi de neuf ?"
          maxlength="100"
          v-model="text"
        ></textarea>
        <button type="submit" :disabled="!trimmedText.length">Publier</button>
      </form>

      <p v-if="loading">Chargement...</p>
      <p v-else-if="!apiPosts.length">Pas de post pour le moment</p>

      <PostCard
        v-for="(post, index) in apiPosts"
        :key="index"
        :post="post"
        @delete="deletePost"
        @like="likePost"
      />

      <!--
    <article v-for="(post, index) in sortedPost" :key="index" class="card">
        <div class="post-header">
          <img
            :src="post.author.avatarUrl"
            alt="pdp"
            width="50px"
            height="50px"
            class="user-img"
          />
          <a>{{ post.author.username }}</a>
        </div>
        <p>{{ post.content }}</p>
    </article>
    --></div>
  </main>
</template>
