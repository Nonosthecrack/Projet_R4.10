<script setup>
//import router from "@/router";
import { computed, ref } from "vue";

import { useRouter } from "vue-router";

const router = useRouter();
//const isloading = ref(false);
const password = ref("");
const username = ref("");
const email = ref("");

function register() {
  // isloading.value = true;
  // console.log({
  //   username: username.value,
  //   email: email.value,
  //   password: password.value,
  // });
  // setTimeout(() => {
  //   router.push("/");
  // }, 1000);
  // router.push("user/Nono");

  fetch("https://posts-crud-api.vercel.app/register", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      username: username.value,
      email: email.value,
      password: password.value,
    }),
  })
    .then((response) => response.json())
    .then((data) => {
      localStorage.setItem("user", JSON.stringify(data));
      router.push("/");
    });
}

const trimmedText = computed(
  () => password.value.trim() && username.value.trim() && email.value.trim()
);
</script>

<style>
input {
  background: var(--color-bg-tertiary);
  border: none;
  border-radius: 10px;
  color: var(--color-text-primary);
  outline: none;
  padding: 1rem;
  margin-bottom: 1rem;
}
button {
  width: 100%;
}
</style>

<template>
  <main>
    <h2>Formulaire de Connexion</h2>
    <form class="card" @submit.prevent="register">
      <div>
        <label for="username">Nom d’utilisateur :</label>
        <input
          type="text"
          id="username"
          name="username"
          required
          v-model="username"
        />
      </div>
      <div>
        <label for="email">Adresse email :</label>
        <input type="email" id="email" name="email" required v-model="email" />
      </div>
      <div>
        <label for="password">Mot de passe :</label>
        <input
          type="password"
          id="password"
          name="password"
          required
          v-model="password"
        />
      </div>
      <div>
        <button type="submit" :disabled="!trimmedText.length">Créer</button>
      </div>
    </form>
  </main>
</template>
