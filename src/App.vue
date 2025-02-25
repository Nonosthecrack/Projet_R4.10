<script setup>
import { computed, ref } from "vue";
const text = ref("");

const trimmedText = computed(() => text.value.trim());

const posts = ref([]);

function addPost() {
  const newPost = {
    id: Math.random().toString(36).substring(2),
    content: trimmedText.value,
    createdAt: new Date(),
    author: {
      username: "Nono",
      avatarUrl:
        "https://media.licdn.com/dms/image/v2/C4D03AQH0TuKfHrOpfQ/profile-displayphoto-shrink_200_200/profile-displayphoto-shrink_200_200/0/1607679351861?e=2147483647&v=beta&t=DQp5RzUGjvkvOkY3dF-EieiqoIV6RVdKdJrpnvaOrpM",
    },
  };
  posts.value.push(newPost);
  text.value = "";
}
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

      <p v-if="!posts.length">Pas de post pour le moment</p>

      <article v-for="(post, index) in posts" :key="index" class="card">
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
    </div>
  </main>
</template>

<style scoped>
.container {
  height: 100vh;
  margin: 0 auto;
  max-width: 640px;
}
.card {
  background-color: var(--color-bg-secondary);
  border-radius: 10px;
  border: 1px solid var(--color-border);
  margin-bottom: 1rem;
}

form {
  display: flex;
  flex-direction: column;
  margin-top: 1rem;
  padding: 1rem 1.5rem;
  width: 100%;
}
textarea {
  background: none;
  border: none;
  color: var(--color-text-primary);
  flex: 1;
  margin-bottom: 1rem;
  outline: none;
  padding: 0.5rem 0;
  resize: none;
  field-sizing: content;
}
button {
  align-self: flex-end;
  background: none;
  border-radius: 10px;
  border: 1px solid var(--color-border);
  color: var(--color-text-primary);
  cursor: pointer;
  font-size: 1rem;
  height: 40px;
  padding: 0 1rem;
}

button:disabled {
  cursor: not-allowed;
  opacity: 0.4;
}

article {
  padding: 0.75rem 1.5rem;
  overflow: hidden;
}

article p {
  white-space: pre-wrap;
}

.user-img {
  border-radius: 50%;
  object-fit: cover;
}

.post-header {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-bottom: 0.75rem;
}
</style>
