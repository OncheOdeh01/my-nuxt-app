<template>
  <nav class="bg-zinc-900 text-white p-4 flex justify-between items-center">

    <h1 class="text-2xl font-bold">
    Dashboard
    </h1>

    <div class="flex gap-4">

      <NuxtLink
        to="/"
        class="hover:text-blue-400"
      >
        Home
      </NuxtLink>

      <NuxtLink
        to="/signup"
        class="hover:text-blue-400"
      >
        Sign-up
      </NuxtLink>

      <NuxtLink
        to="/login"
        class="hover:text-blue-400"
      >
        Login
      </NuxtLink>

      <NuxtLink
        to="/dashboard"
        class="hover:text-blue-400"
      >
        Dashboard
      </NuxtLink>

    </div>

  </nav>
</template>