<template>
  <div>
    <div v-if="auth === false">
      <login @enter="checkLogin" />
    </div>
    <div v-else>
      <cajero :usuario="usuarioIngresado" />
    </div>
  </div>
</template>

<script>
import Cajero from "./components/Cajero.vue";
import Login from "./components/Login.vue";
export default {
  name: "app",
  components: {
    Cajero,
    Login
  },

  data() {
    return {
      auth: false,
      usuarios: [],
      usuarioIngresado: {}
    };
  },
  mounted() {
    this.loadUsuarios();
    // ejecuta el evento
  },
  methods: {
    loadUsuarios: async function() {
      try {
        const data = await fetch("./usuarios.json");
        this.usuarios = await data.json();
      } catch (error) {
        throw error;
      }
    },
    //uso como parametro contraseña q viene del login
    checkLogin(contraseña, nombreUsuario) {
      const cantidadUsuarios = this.usuarios.length;
      for (let i = 0; i < cantidadUsuarios; i++) {
        // la condicion del for si son mas condiciones puede ir con ( )
        console.log(this.usuarios[i]);

        if (
          parseInt(contraseña) == parseInt(this.usuarios[i].contraseña) &&
          this.usuarios[i].usuario.toLowerCase() == nombreUsuario.toLowerCase()
        ) {
          this.usuarioIngresado = this.usuarios[i];
          this.auth = true;
          console.log(this.usuarioIngresado);
        }
      }
    }
    /* let contra = parseInt(contraseña)
      if( parseInt(contra)===  1122 ){
        this.auth = true} 
       */

    //   this.validar();
    // } else {
    // let pass = parseInt(txt);
    // if (parseInt(pass) === 1122) {
    //  this.auth = true;
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Source+Sans+Pro");
body {
  margin: 0;

  font-family: "Source Sans Pro", sans-serif;

  background-color: #2a333d;
}
</style>
