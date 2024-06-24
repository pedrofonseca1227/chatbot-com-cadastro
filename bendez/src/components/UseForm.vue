<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const newUserEmail = ref('');
const newUserPassword = ref('');
const editingUser = ref(null);  

const router = useRouter();  
const login = () => {
  if (newUserEmail.value && newUserPassword.value) {

    newUserEmail.value = '';
    newUserPassword.value = '';

    router.push({ name: 'dashboard' });
  } else {
    if (!newUserEmail.value) {
      document.getElementById('errorEmail').hidden = false;
    }
    if (!newUserPassword.value) {
      document.getElementById('errorSenha').hidden = false;
    }
  }
};
</script>

<template>
  <div class="form">
    <div v-if="editingUser">
      <h2>Editar Usuário</h2>
      <input type="text" v-model="editingUser.email" placeholder="E-mail..." />
      <button @click="updateUser">Salvar</button>
      <button @click="cancelEdit">Cancelar</button>
    </div>
    <div v-else>
      <div class="login-form" id="login-form">
        <h2>Cadastre-se</h2>
        <input type="text" v-model="newUserEmail" placeholder="E-mail..." id="txtEmail"/>
        <span class="error" id="errorEmail" hidden>Informe este campo</span>
        <input type="password" v-model="newUserPassword" placeholder="Senha..." id="txtSenha"/>
        <span class="error" id="errorSenha" hidden>Informe este campo</span>
        <button @click="login">Entrar</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.login-form span {
  color: red;
}

.login-form {
  background: #FFFFFF;
  max-width: 360px;
  margin: 0 auto;
  padding: 45px;
  text-align: center;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24);
  margin-top: 100px;
}

.form input {
  font-family: "Roboto", sans-serif;
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
  background: #EF3B3A;
}

.error {
  color: #EF3B3A;
  margin: -15px;
}
</style>
