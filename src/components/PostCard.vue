<script setup>
import { TrashIcon, HeartIcon } from "@heroicons/vue/24/outline";
import { HeartIcon as SolidHeartIcon } from "@heroicons/vue/24/solid";
defineProps(["post"]);
const emit = defineEmits(["delete", "like"]);
</script>

<template>
  <article class="card">
    <header>
      <img
        :src="post.author.avatarUrl"
        alt="pdp"
        width="50px"
        height="50px"
        class="avatar"
      />
      <a>{{ post.author.username }}</a>
    </header>
    <p>{{ post.content }}</p>
    <footer>
      <button class="btn-icon" @click="emit('delete', post.id)">
        <TrashIcon />
      </button>
      <button class="btn-icon" @click="emit('like', post.id)">
        <HeartIcon v-if="!post.liked" />
        <SolidHeartIcon v-if="post.liked" />
      </button>
    </footer>
  </article>
</template>
