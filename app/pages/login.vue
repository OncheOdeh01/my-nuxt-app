<script setup>
import { ref } from 'vue'
import axios from 'axios'

const email = ref('')
const password = ref('')
const error = ref('')
const loading = ref(false)

const login = async () => {

  error.value = ''

  if (!email.value || !password.value) {
    error.value = 'Please fill all fields'
    return
  }

  loading.value = true

  try {

    const response = await axios.get(
      'https://jsonplaceholder.typicode.com/users'
    )

    const users = response.data

    const foundUser = users.find(
      user => user.email === email.value
    )

    if (!foundUser) {
      error.value = 'User not found'
      loading.value = false
      return
    }

    const loggedInUser = {
      id: foundUser.id,
      name: foundUser.name,
      email: foundUser.email
    }

    localStorage.setItem(
      'loggedInUser',
      JSON.stringify(loggedInUser)
    )

    navigateTo('/dashboard')

  } catch (err) {

    error.value = 'Something went wrong'

  } finally {

    loading.value = false
  }
}
</script>

<template>
  <div class="min-h-screen bg-zinc-950 flex items-center justify-center">

    <div class="bg-zinc-900 p-8 rounded-2xl w-[400px]">

      <h1 class="text-white text-4xl font-bold mb-2 text-center">
        Login
      </h1>

      <p class="text-zinc-400 text-center mb-8">
        Welcome back
      </p>

      <div
        v-if="error"
        class="bg-red-500/20 border border-red-500 text-red-300 p-3 rounded mb-4"
      >
        {{ error }}
      </div>

      <input
        v-model="email"
        type="email"
        placeholder="Email"
        class="w-full p-3 rounded bg-zinc-800 text-white mb-4 outline-none"
      />

      <input
        v-model="password"
        type="password"
        placeholder="Password"
        class="w-full p-3 rounded bg-zinc-800 text-white mb-6 outline-none"
      />

      <button
        @click="login"
        :disabled="loading"
        class="w-full bg-blue-500 hover:bg-blue-600 disabled:opacity-50 text-white p-3 rounded font-bold transition"
      >

        <span v-if="loading">
          Logging in...
        </span>

        <span v-else>
          Login
        </span>

      </button>

      <div class="mt-6 text-center">

        <NuxtLink
          to="/signup"
          class="text-blue-400 hover:underline"
        >
          Create account
        </NuxtLink>

      </div>

    </div>

  </div>
</template>