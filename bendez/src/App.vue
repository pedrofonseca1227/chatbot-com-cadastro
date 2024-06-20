<script setup>
import { ref } from 'vue';

// Dados de exemplo dos usuários cadastrados
const users = ref([
  { id: 1, email: 'user1@example.com' },
  { id: 2, email: 'user2@example.com' }
]);

const editingUser = ref(null);
const newUserEmail = ref('');
const newUserPassword = ref('');

function deleteUser(userId) {
  users.value = users.value.filter(user => user.id !== userId);
}

function editUser(user) {
  editingUser.value = { ...user };
}

function updateUser() {
  const index = users.value.findIndex(user => user.id === editingUser.value.id);
  if (index !== -1) {
    users.value[index] = editingUser.value;
  }
  editingUser.value = null;
}

function cancelEdit() {
  editingUser.value = null;
}

function login() {
  if (newUserEmail.value && newUserPassword.value) {
    const newUser = {
      id: users.value.length + 1,
      email: newUserEmail.value
    };
    users.value.push(newUser);
    newUserEmail.value = '';
    newUserPassword.value = '';
  } else {
    if (!newUserEmail.value) {
      document.getElementById('errorEmail').hidden = false;
    }
    if (!newUserPassword.value) {
      document.getElementById('errorSenha').hidden = false;
    }
  }
}

function cancel() {
  // Função de cancelar aqui
}
</script>

<template>
  <header> 
    <div class="header" id="header">
      <div class="logo_header">
        <img src="https://fulltime.com.br/wp-content/themes/fulltime-brasil-1/images/logo.png" class="logo_ft" alt="FullTime ">
      </div>
      <div class="navigation_header">
        <a href="#" class="ativo"> Home </a>
        <a href="#" class="ativo"> Sobre Nós </a>
        <a href="#" class="ativo"> Blog </a>
        <a href="#" class="ativo"> Soluções </a>
        <a href="#" class="ativo"> Fale Conosco </a>
        <a href="#" class="ativo"> Agenda </a>
        <a href="#" class="ativo"> FAQ </a>
      </div>
    </div>
  </header>
  
  <div class="login-page">
    <div class="form">
      <div v-if="editingUser">
        <h2>Editar Usuário</h2>
        <input type="text" v-model="editingUser.email" placeholder="E-mail..." />
        <button @click="updateUser">Salvar</button>
        <button @click="cancelEdit">Cancelar</button>
      </div>
      <div v-else>
        <div class="login-form" id="login-form">
          <input type="text" v-model="newUserEmail" placeholder="E-mail..." id="txtEmail"/>
          <span class="error" id="errorEmail" hidden>Informe este campo</span>
          <input type="password" v-model="newUserPassword" placeholder="Senha..." id="txtSenha"/>
          <span class="error" id="errorSenha" hidden>Informe este campo</span>
          <button @click="login">Entrar</button>
        </div>
      </div>
      <div id="loading" hidden>
        <h2>Carregando...</h2>
        <button @click="cancel">Cancelar</button>
      </div>
    </div>
    
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
  </div>
</template>

<style scoped>
@import url(https://fonts.googleapis.com/css?family=Roboto:300);

.logo_ft {
  width: 100px;
}
.header {
  flex-direction: row;
  display: flex;
  align-items: center;
  background-color: rgba(48, 46, 46, 0.989);
  height: 100px;
  justify-content: space-between;
  padding: 0 10%;
}

.navigation_header {
  flex-direction: row;
  display: flex;
  align-items: center;
  margin-right: 300px;
  gap: 3em;
}

.navigation_header a {
  text-decoration: none;
  color: #fff;
  transition: 1s;
  font-weight: bold;
}

.app {
  background-color: #E3F2FD; /* Updated background color */
}
.login-page {
  width: 360px;
  padding: 8% 0 0;
  margin: auto;
}

.form {
  position: relative;
  z-index: 1;
  background: #FFFFFF;
  max-width: 360px;
  margin: 0 auto 100px;
  padding: 45px;
  text-align: center;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24);
}

.form input {
  font-family: "Roboto", sans-serif;
  outline: 0;
  background: #f2f2f2;
  width: 100%;
  border: 0;
  margin: 0 0 15px;
  padding: 15px;
  box-sizing: border-box;
  font-size: 14px;
}

.form button {
  font-family: "Roboto", sans-serif;
  text-transform: uppercase;
  outline: 0;
  background: #af4c4c;
  width: 100%;
  border: 0;
  padding: 15px;
  color: #FFFFFF;
  font-size: 14px;
  -webkit-transition: all 0.3 ease;
  transition: all 0.3 ease;
  cursor: pointer;
}

.form button:hover,.form button:active,.form button:focus {
  background: #a04343;
}

.container {
  position: relative;
  z-index: 1;
  max-width: 300px;
  margin: 0 auto;
}

.error {
  color: #EF3B3A;
  margin: -15px;
}

.user-list {
  background: #FFFFFF;
  max-width: 360px;
  margin: 0 auto;
  padding: 45px;
  text-align: center;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24);
  margin-bottom: 100px;
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
