<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import Button from 'primevue/button'
import InputText from 'primevue/inputtext'
import Card from 'primevue/card'

const students = ref([])
const newStudent = ref({ name: '', age: null })
const apiBase = 'http://localhost:3000/students'

const fetchStudents = async () => {
  const res = await axios.get(apiBase)
  students.value = res.data
}

const addStudent = async () => {
  await axios.post(apiBase, newStudent.value)
  newStudent.value = { name: '', age: null }
  fetchStudents()
}

const updateStudent = async (student) => {
  await axios.put(`${apiBase}/${student.id}`, student)
  fetchStudents()
}

const deleteStudent = async (id) => {
  await axios.delete(`${apiBase}/${id}`)
  fetchStudents()
}

onMounted(fetchStudents)
</script>

<template>
  <Card class="max-w-xl mx-auto mt-10">
    <template #title>Student Manager</template>

    <div class="flex gap-2 mb-4">
      <InputText v-model="newStudent.name" placeholder="Name" />
      <InputText v-model.number="newStudent.age" placeholder="Age" />
      <Button label="Add" icon="pi pi-plus" @click="addStudent" />
    </div>

    <ul>
      <li v-for="s in students" :key="s.id" class="flex gap-2 mb-2">
        <InputText v-model="s.name" />
        <InputText v-model.number="s.age" />
        <Button label="Update" icon="pi pi-check" severity="success" @click="updateStudent(s)" />
        <Button label="Delete" icon="pi pi-trash" severity="danger" @click="deleteStudent(s.id)" />
      </li>
    </ul>
  </Card>
</template>