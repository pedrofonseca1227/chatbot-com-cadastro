<script setup>
import { ref } from 'vue';

const users = ref([
  { id: 1, email: 'user1@example.com' },
  { id: 2, email: 'user2@example.com' }
]);

const deleteUser = (userId) => {
  users.value = users.value.filter(user => user.id !== userId);
};
</script>

<template>
  <div class="user-list">
    <h2>Usuários Cadastrados</h2>
    <ul>
      <li v-for="user in users" :key="user.id">
        {{ user.email }}
        <button @click="editUser(user)">Editar</button>
        <button @click="deleteUser(user.id)">Excluir</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.user-list {
  background: #FFFFFF;
  max-width: 360px;
  margin: 0 auto;
  padding: 45px;
  text-align: center;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24);
  margin-top: 100px;
}

.user-list ul {
  list-style: none;
  padding: 0;
}

.user-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 10px 0;
  padding: 10px;
  background: #f2f2f2;
  border-radius: 5px;
}

.user-list button {
  margin-left: 10px;
  padding: 5px 10px;
  cursor: pointer;
  color: #fff;
  background-color: #af4c4c;
  
}
</style>
