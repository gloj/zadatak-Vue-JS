
<template>
  <div class="hello">
    <div class="header">
    <h1>Users CRUD</h1>
    <input type="text" class="form-control" placeholder="Search by name"
          v-model="searchString"/>
    <button @click="onSearch" class="dugme">Search</button>
  </div>
  <br>
     <div class="body" id="app">
      <table class="table">
        <thead>
          <tr>
            <th>Avatar</th>
            <th>Name</th>
            <th>Email</th>
            <th>Created At</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(user,id) in users" :key="id">
            <td> <img class="avatar" :src="user.avatar" alt="Profile picture"/></td>
            <td v-text="user.name"></td>
            <td v-text="user.email"></td>
            <td v-text="user.createdAt"></td>
            <td ><button v-on:click="openEditUserModal(user)" class="edit">EDIT</button>
            <br><br>
             <button v-on:click="deleteUser(user.id)" class="delete">DELETE</button>
             </td>
          </tr>
        </tbody>
      </table>
    </div>
     
  <EditUserModal
      v-show="isModalVisible"
      @close="closeEditUserModal"
      :user="userForEdit"
    />
  </div>
 
</template>

<script>


import axios from 'axios'
import EditUserModal from './EditUserModal.vue'


export default {
  name: 'Users',
  components: {
    EditUserModal
   },
  props: {
    msg: String,
    userForEdit: Object
  },
  
  data () {
    return {
      users: null,
      isModalVisible: false,
      searchString:"",
      
    }
  },
  
  mounted () {
    
    axios
      .get('https://6103057679ed680017482413.mockapi.io/users/')
      .then(response => (this.users = response.data))
  
  },
   
  methods: {
    onSearch: function(){
    axios
      .get('https://6103057679ed680017482413.mockapi.io/users?name='+ this.searchString)
      .then(response => (this.users = response.data))
  },

    deleteUser: function(id){
      axios
      .delete('https://6103057679ed680017482413.mockapi.io/users/' + id)
      .then(response => {
        if(response.status == 200){
          let index = this.users.map(u => u.id).indexOf(id)
          this.users.splice(index, 1)
        }
      }) 
    },
     openEditUserModal: function (user) {
        this.userForEdit= user;
        this.isModalVisible = true;
        
    },
    closeEditUserModal() {
      this.isModalVisible = false;
      },
    
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}

.table, td, th {
  border: 1px solid #dddddd;
  padding: 10px;;
  text-align: center;
} 
.table{
  position: relative;
  align-content: space-around;
  top: 85px;
  margin: auto;
  border-collapse: collapse;
 
}

.dugme{
  background-color: #363636;
  text-align: center;
  color: white;
  width: 5em;
  border-radius: 0 3px 3px 0;
  height: 2.1em;
  font-weight: lighter;
}
.dugme, .form-control{
  border-style: hidden;  
}
.form-control{
  border-radius: 3px 0 0 3px;
  padding-left: 8px;
  height: 2em;
  width: 20%;
}
button{
  color:white;
   font-weight: lighter;
}
h1{
  color:#363636;
}
.header{
position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 20%;
    background-color: pink;
    padding: 15px;
    z-index: 10;
}
th{
 position: sticky;
  top: 0;
  
}
.avatar{
  width: 60px;
  height: 60px;
}
.edit{
  background-color:dodgerblue;
  border-style: hidden;
  text-justify: center;
  width: 5em;
  text-align: center;
  font-size: 13px;
  padding-top: 3px;
  
}
.delete{
  
  background-color: firebrick;
  width: 5em;
  border-style: hidden;
  font-size: 13px;
  padding: 3px;
  text-align: center;
  
}
</style>

