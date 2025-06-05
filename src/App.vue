<script setup>
import { ref } from 'vue'
import contactsData from './contacts.json'

const contacts = ref(contactsData.slice(0, 5))

function addRandomContact() {
  const remainingContacts = contactsData.filter(
    c => !contacts.value.some(existing => existing.id === c.id)
  )
  if (remainingContacts.length === 0) return
  const randomIndex = Math.floor(Math.random() * remainingContacts.length)
  contacts.value.push(remainingContacts[randomIndex])
}

function sortByName() {
  contacts.value = [...contacts.value].sort((a, b) => a.name.localeCompare(b.name))
}

function sortByPopularity() {
  contacts.value = [...contacts.value].sort((a, b) => b.popularity - a.popularity)
}

function deleteContact(id) {
  contacts.value = contacts.value.filter(contact => contact.id !== id)
}
</script>

<template>
  <div>
    <h1>Contacts</h1>

    <div style="margin-bottom: 10px;">
      <button @click="addRandomContact">Add Random Contact</button>
      <button @click="sortByName">Sort by Name</button>
      <button @click="sortByPopularity">Sort by Popularity</button>
    </div>

    <table>
      <thead>
        <tr>
          <th>Picture</th>
          <th>Name</th>
          <th>Popularity</th>
          <th>Won an Oscar</th>
          <th>Won an Emmy</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in contacts" :key="contact.id">
          <td><img :src="contact.pictureUrl" :alt="contact.name" width="50" /></td>
          <td>{{ contact.name }}</td>
          <td>{{ contact.popularity.toFixed(2) }}</td>
          <td>{{ contact.wonOscar ? '🏆' : '' }}</td>
          <td>{{ contact.wonEmmy ? '🏆' : '' }}</td>
          <td><button @click="deleteContact(contact.id)">Delete</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
table {
  border-collapse: collapse;
  width: 100%;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: center;
}

th {
  background-color: #f2f2f2;
}

button {
  padding: 5px 10px;
  cursor: pointer;
  border: none;
  background-color: #3498db;
  color: white;
  border-radius: 5px;
  margin-right: 5px;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #2980b9;
}

img {
  border-radius: 50%;
}
</style>

