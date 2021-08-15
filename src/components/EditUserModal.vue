<template>
  <div class="modal-backdrop">
    <div class="modal">
      <header class="modal-header">
        <slot name="header">
          Edit pleace!
        </slot>
        <button
          type="button"
          class="btn-close"
          @click="close"
        >
          x
        </button>
      </header>

      <section class="modal-body">
        <slot name="body">
<p>Name</p>
<input type="text" class="form-control" placeholder="Search by title" v-model="user.name"/>
<p>Email</p>
<input type="text" class="form-control" placeholder="Search by title" v-model="user.email"/>

        </slot>
       </section>

      <footer class="modal-footer">
       
        <button
          type="button"
          class="btn"
          @click="onSaveEdit"
        >
          Save
        </button>
      </footer>
    </div>
  </div>
</template>


<script>

import axios from 'axios'

  export default {
    name: 'EditUserModal',
      props: {
      user: Object,
  
  },
    methods: {
      close() {
        this.$emit('close');
      },
      onSaveEdit(){
          axios
        .put('https://6103057679ed680017482413.mockapi.io/users/' + this.user.id, this.user)
        .then(response => {
          if(response.status == 200){
             this.user.createdAt=Date.now();
            this.close()
          }
        })
      }
    },
  };
</script>

<style>
  .modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
    
  }

  .modal {
    background: #FFFFFF;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
  }

  .modal-header,
  .modal-footer {
    padding: 15px;
    display: flex;
  }

  .modal-header {
    position: relative;
    border-bottom: 1px solid #ffc1cd;
    color: #ffc1cd;
    background-color: #363636;
    justify-content: space-between;
  }

  .modal-footer {
    border-top: 1px solid #ffc1cd;
    flex-direction: column;
    justify-content: flex-end;
    background-color: #363636;
  }

  .modal-body {
    position: relative;
    padding: 20px 10px;
    background-color: #363636;
    color: #ffc1cd;
  }

  .btn-close {
    position: absolute;
    top: 0;
    right: 0;
    border: none;
    font-size: 20px;
    padding: 10px;
    cursor: pointer;
    font-weight: bold;
    color: #ffc1cd;
    background: transparent;
  }

  .btn {
    color: #ffc1cd;
    background-color: #363636;
    border: 1px solid #ffc1cd;
    border-radius: 2px;
  }
</style>
