<template>
  <h1>Manage Users</h1>
  <form action="" class="form" @submit.prevent="handleLogin">
    <label for="name">Name</label>
    <input type="text" name="name" id="name" v-model="form.name">

    <label for="surname">Surname</label>
    <input type="text" name="surname" id="surname" v-model="form.surname">

    <label for="email">Email</label>
    <input type="email" name="email" id="email" v-model="form.email">

    <label for="pet-select">Choose a role:</label>

    <select name="role" id="role" v-model="form.role">
      <option value="">--choose Role--</option>
      <option value="admin">Admin</option>
      <option value="reader">Reader</option>
      <option value="editor">Editor</option>
    </select>
    <label for="enabled"> Enabled</label>
    <input type="checkbox" id="enabled" name="enabled" value="enabled" v-model="form.enabled">

    <input type="submit" value="ADD" @click="submitRole()">
  </form>
  
  <table>
    <thead>
      <tr>
        <th>Id</th>
        <th>Name</th>
        <th>Surname</th>
        <th>Email</th>
        <th>Role</th>
        <th>Enabled</th>
        <th>Actions</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, i) in listUsers" :key="i">
        <td>{{ item.id }}</td>
        <td>{{ item.name }}</td>
        <td>{{ item.email }}</td>
        <td>{{ item.surname }}</td>
        <td>{{ item.role }}</td>
        <td>{{ item.enabled }}</td>
        <td><button @click="deleteId(item.id)">Delete</button></td>
      </tr>
    </tbody>
  </table>
</template>

<script>
// import FilterList from '@/components/FilterList.vue';
import axios from 'axios';


export default {
  components: {

  },
  data() {
    return {
      form: {
        id: '',
        name: '',
        surname: '',
        email: '',
        role: '',
        enabled: false,
      },
      listUsers: []
    }
  },
  computed: {

  },
  methods: {
    async submitRole() {
      console.log(this.name)
      await axios.post('http://localhost:3000/users', this.form).then(()=>{
        this.fetchUsers()
      })
    },
     deleteId(id) {
       axios.delete(`http://localhost:3000/users/${id}`, this.form).then (()=>{
        this.fetchUsers()
       })
    },
    async fetchUsers() {
      try {
        const resp = await axios.get('http://localhost:3000/users')
        this.listUsers = resp.data
      } catch (err) {
        console.log(err)
      }
    },
  },
  created() {
    this.fetchUsers()
  }
}
</script>


<style scoped></style>
