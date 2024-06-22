<template>
  <header> 
    <div class="header">
      <div class="logo_header">
        <img src="https://fulltime.com.br/wp-content/themes/fulltime-brasil-1/images/logo.png" class="logo_ft" alt="FullTime ">
      </div>
      <div class="navigation_header">
        <router-link to="/">Home</router-link>
        <a href="#" class="ativo"> Sobre Nós </a>
        <a href="#" class="ativo"> Blog </a>
        <a href="#" class="ativo"> Soluções </a>
        <a href="#" class="ativo"> Fale Conosco </a>
        <a href="#" class="ativo"> Agenda </a>
        <router-link to="/dashboard" hidden>SAC Inteligente</router-link>
        <a href="#" class="ativo"> FAQ </a>
      </div>
    </div>
  </header>
  <router-view></router-view>
</template>


<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: rgba(48, 46, 46, 0.989);
  height: 100px;
  width: 100%;
  padding: 0 10%;
  position: fixed; 
  top: 0;
  left: 0;
  z-index: 1000; 
}

.logo_ft {
  width: 100px;
}

.navigation_header {
  display: flex;
  align-items: center;
  gap: 2em;
  padding: 0 150px;
}

.navigation_header a {
  text-decoration: none;
  color: #fff;
  transition: color 0.3s ease;
  font-weight: bold;
}
</style>

