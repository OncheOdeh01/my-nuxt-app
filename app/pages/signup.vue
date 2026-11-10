<script setup>
import { ref } from 'vue'
import axios from 'axios'

const username = ref('')
const email = ref('')
const password = ref('')

const signup = async () => {

  if (!username.value || !email.value || !password.value) {
    alert('Fill all fields')
    return
  }

  try {
    const newUser = {
      id: Date.now(),
      username: username.value,
      name: username.value,
      email: email.value,
      source: 'registeredUsers'
    }

    const existingUsers = JSON.parse(
      localStorage.getItem('registeredUsers') || '[]'
    )

    existingUsers.push(newUser)

    localStorage.setItem(
      'registeredUsers',
      JSON.stringify(existingUsers)
    )

    console.log('Registered user:', newUser)

    alert('User Created')

    await navigateTo('/login')

  } catch (error) {
    console.error('Signup error:', error)
    alert('Something went wrong')
  }
}
</script>

<template>
  <div class="min-h-screen flex items-center justify-center bg-zinc-950">

    <div class="bg-zinc-900 p-8 rounded-2xl w-[350px]">

      <h1 class="text-white text-3xl mb-6 font-bold text-center">
        Sign Up
      </h1>

      <input
        v-model="username"
        placeholder="Username"
        class="w-full p-3 mb-4 rounded bg-zinc-800 text-white"
      />

      <input
        v-model="email"
        placeholder="Email"
        class="w-full p-3 mb-4 rounded bg-zinc-800 text-white"
      />

      <input
        v-model="password"
        type="password"
        placeholder="Password"
        class="w-full p-3 mb-6 rounded bg-zinc-800 text-white"
      />

      <button
        @click="signup"
        class="w-full bg-blue-500 hover:bg-blue-600 p-3 rounded text-white font-bold"
      >
        Create Account
      </button>

    </div>

  </div>
</template>