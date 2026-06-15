<template>
  <div class="min-h-screen bg-zinc-950 text-white flex flex-col items-center justify-center">

    <h1 class="text-6xl font-bold mb-6">
      Welcome
    </h1>

    <p class="text-zinc-400 text-xl mb-10">
      
    </p>

    <div class="flex gap-4">

      <NuxtLink
        to="/signup"
        class="bg-blue-500 hover:bg-blue-600 px-6 py-3 rounded-xl"
      >
        Sign Up
      </NuxtLink>

      <NuxtLink
        to="/login"
        class="bg-zinc-800 hover:bg-zinc-700 px-6 py-3 rounded-xl"
      >
        Login
      </NuxtLink>

    </div>

  </div>
</template>