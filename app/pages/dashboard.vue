<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const users = ref([])

const fetchUsers = async () => {

  const response = await axios.get(
    'https://jsonplaceholder.typicode.com/users'
  )

  users.value = response.data
}

const deleteUser = async (id) => {

  await axios.delete(
    `https://jsonplaceholder.typicode.com/users/${id}`
  )

  users.value = users.value.filter(
    user => user.id !== id
  )
}

onMounted(() => {
  fetchUsers()
})
</script>

<template>
  <div class="min-h-screen bg-zinc-950 text-white p-10">

    <h1 class="text-4xl font-bold mb-8">
      Dashboard
    </h1>

    <div class="bg-zinc-900 p-6 rounded-2xl overflow-auto">

      <table class="w-full">

        <thead>
          <tr class="border-b border-zinc-700">
            <th class="p-3 text-left">Name</th>
            <th class="p-3 text-left">Email</th>
            <th class="p-3 text-left">Action</th>
          </tr>
        </thead>

        <tbody>

          <tr
            v-for="user in users"
            :key="user.id"
            class="border-b border-zinc-800"
          >
            <td class="p-3">
              {{ user.name }}
            </td>

            <td class="p-3">
              {{ user.email }}
            </td>

            <td class="p-3">

              <button
                @click="deleteUser(user.id)"
                class="bg-red-500 px-3 py-1 rounded"
              >
                Delete
              </button>

            </td>
          </tr>

        </tbody>

      </table>

    </div>

  </div>
</template>