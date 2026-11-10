<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const users = ref([])
const loading = ref(true)

const fetchUsers = async () => {
  try {
    // 1. JSONPlaceholder
    const response = await axios.get(
      'https://jsonplaceholder.typicode.com/users'
    )

    const apiUsers = response.data.map(user => ({
      ...user,
      source: 'Dummy API'
    }))

    // 2. locally registered users
    const registeredUsers = JSON.parse(
      localStorage.getItem('registeredUsers') || '[]'
    )

    // 3. Combine both lists
    users.value = [...registeredUsers, ...apiUsers]

  } catch (error) {
    console.error('Error fetching users:', error)
  } finally {
    loading.value = false
  }
}

const deleteUser = async (user) => {
  try {
    if (user.source === 'Registered User') {
      
      const registeredUsers = JSON.parse(
        localStorage.getItem('registeredUsers') || '[]'
      )

      const updatedUsers = registeredUsers.filter(
        item => item.id !== user.id
      )

      localStorage.setItem(
        'registeredUsers',
        JSON.stringify(updatedUsers)
      )

    } else {
      // Simulate deleting a Dummy API user
      await axios.delete(
        `https://jsonplaceholder.typicode.com/users/${user.id}`
      )
    }

    // Update dashboard immediately
    users.value = users.value.filter(
      item => !(item.id === user.id && item.source === user.source)
    )

  } catch (error) {
    console.error('Delete error:', error)
    alert('Unable to delete user')
  }
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

      <p v-if="loading" class="text-zinc-400">
        Loading users...
      </p>

      <table v-else class="w-full">

        <thead>
          <tr class="border-b border-zinc-700">
            <th class="p-3 text-left">Name</th>
            <th class="p-3 text-left">Email</th>
            <th class="p-3 text-left">Source</th>
            <th class="p-3 text-left">Action</th>
          </tr>
        </thead>

        <tbody>
          <tr
            v-for="user in users"
            :key="`${user.source}-${user.id}`"
            class="border-b border-zinc-800"
          >
            <td class="p-3">
              {{ user.name || user.username }}
            </td>

            <td class="p-3">
              {{ user.email }}
            </td>

            <td class="p-3">
              <span
                :class="user.source === 'Registered User'
                  ? 'bg-green-600'
                  : 'bg-blue-600'"
                class="px-3 py-1 rounded-full text-xs"
              >
                {{ user.source }}
              </span>
            </td>

            <td class="p-3">
              <button
                @click="deleteUser(user)"
                class="bg-red-500 hover:bg-red-600 px-3 py-1 rounded"
              >
                Delete
              </button>
            </td>
          </tr>

          <tr v-if="users.length === 0">
            <td colspan="4" class="p-4 text-center text-zinc-400">
              No users found.
            </td>
          </tr>
        </tbody>

      </table>

    </div>
  </div>
</template>