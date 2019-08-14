<template>
  <div>
    <div class="header">
      <img class="logo" src="../assets/logo.png" />
      <div class="align-left">
        <span id="nombre">Bienvenido/a</span>
        <img class="avatar" src="../assets/avatar.svg" />
      </div>
    </div>
    <div class="white-container">
      <div class="menu-container">
        <h1 class="tu-cuenta">Login</h1>
        <input type="text" @keyup.enter="enter()" v-model="nombreUsuario" />
        <input type="password" @keyup.enter="enter()" v-model="contraseña" />
        <button class="links" @click="enter()">Entrar</button>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: "Login",

  data() {
    return {
      nombreUsuario: "",
      contraseña: ""
    };
  },
  methods: {
    enter() {
      this.$emit("enter", this.contraseña, this.nombreUsuario);
    }
  }
};
</script>

