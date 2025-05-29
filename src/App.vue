<template>
  <div>
    <h1>IronContact</h1>

    
    <div class="buttons">
      <button @click="addRandomContact" :disabled="availableContacts.length === 0">
        Añadir contacto aleatorio
      </button>
      <button @click="sortByName">Ordenar por nombre</button>
      <button @click="sortByPopularity">Ordenar por popularidad</button>
    </div>

    
    <table>
      <thead>
        <tr>
          <th>Foto</th>
          <th>Nombre</th>
          <th>Popularidad</th>
          <th>Ganó un Oscar</th>
          <th>Ganó un Emmy</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in contacts" :key="contact.id">
          <td><img :src="contact.pictureUrl" :alt="contact.name" width="80" /></td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity.toFixed(2) }}</td>
          <td>{{ contact.wonOscar ? '🏆' : '' }}</td>
          <td>{{ contact.wonEmmy ? '🏆' : '' }}</td>
          <td>
            <button class="delete" @click="deleteContact(contact.id)">🗑️ Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import allContacts from './contacts.json'


const contacts = ref(allContacts.slice(0, 5))


const availableContacts = computed(() => {
  return allContacts.filter(
    (c) => !contacts.value.some((shown) => shown.id === c.id)
  )
})


function addRandomContact() {
  if (availableContacts.value.length === 0) return
  const randomIndex = Math.floor(Math.random() * availableContacts.value.length)
  const randomContact = availableContacts.value[randomIndex]
  contacts.value.push(randomContact)
}


function sortByName() {
  contacts.value = [...contacts.value].sort((a, b) =>
    a.name.localeCompare(b.name)
  )
}


function sortByPopularity() {
  contacts.value = [...contacts.value].sort((a, b) =>
    b.popularity - a.popularity
  )
}


function deleteContact(id) {
  contacts.value = contacts.value.filter(contact => contact.id !== id)
}
</script>

<style scoped>
.buttons {
  margin-bottom: 15px;
  display: flex;
  gap: 10px;
}

button {
  padding: 8px 14px;
  background-color: #3b82f6;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

button.delete {
  background-color: #ef4444;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th,
td {
  border: 1px solid #ccc;
  padding: 10px;
  text-align: center;
}

img {
  border-radius: 8px;
}
</style>
