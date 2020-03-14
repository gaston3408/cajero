<template>
  <div>
    <div class="header">
      <img class="logo" src="../assets/logo.png" />
      <div class="align-left">
        <span id="nombre">Bienvenido/a {{usuario.usuario}}</span>
        <img class="avatar" src="../assets/avatar.svg" />
      </div>
    </div>
    <div class="white-container">
      <div class="menu-container">
        <h1 class="tu-cuenta">Tu cuenta</h1>
        <button class="links" @click="extraerDinero()">Extraer dinero</button>
        <button class="links" @click="depositarDinero()">Depositar dinero</button>
        <button class="links" @click="mostrar ++">Pagar Servicio</button>
        <span v-if="mostrar %2">
          <div>
            <button class="links" v-for="servicio in servicios" @click="pagarServicio(servicio)" :key="servicio.id">
              <!-- en el v-for podemos seleccionar el servicio q se selecciono con un parametro llamado servicio -->
              <small>{{servicio.serv}}</small>
            </button>
          </div>
        </span>

        <button class="links" @click="transferirDinero()">Transferir dinero</button>
        <button class="links" @click="cambiarLimiteDeExtraccion()">Cambiar límite de extracción</button>
      </div>
      <div class="green-container">
        <div class="cuenta-info">
          <p>Saldo en tu cuenta</p>
          <h3 id="saldo-cuenta">${{usuario.saldo}}</h3>
          <p id="limite-extraccion">
            Tu límite de extracción es:
            ${{usuario.limite}}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { async } from "q";
export default {
  name: "Cajero",
  props: ["usuario"],
  data() {
    return {
      auth: false,
      mostrar: 2,
      //Saldo: 40000,
      //LimiteDeExtraccion: 5000,
      //Nombre: "GASTON CAMAÑO",
      servicios: []
    };
  },
  mounted() {
    this.loadServicios();
  },
  //this.validar();
  //this.ingresarNombre();

  methods: {
    loadServicios: async function() {
      try {
        const data = await fetch("./servicios.json");
        this.servicios = await data.json();
      } catch (error) {
        throw error;
      }
    },

    //validar() {
    // let txt = prompt("Ingrese la clave:");
    // if (txt == null || txt == "") nulo o vacio {
    // "User cancelled the prompt."
    //   this.validar();
    // } else {
    // let pass = parseInt(txt);
    // if (parseInt(pass) === 1122) {
    //  this.auth = true;
    // }
    //}
    //},
    // ingresarNombre() {
    // let txt = prompt("Ingrese nombre y apellido");
    // let usuario = txt;
    // if (usuario) {
    //  this.Nombre = usuario;
    // }
    // },
    pagarServicio(servicio) {
      // SE PUEDE PASAR POR PARAMETRO EL SERVICIO AL Q ESTOY ACCEDIENDO ..ASI ME AHORRO CODIGO
    
      //cuando se pasa por parametro no se le pone el this
      let total = servicio.monto;
      let minimo = servicio.montoMinimo;
      //para mostrar en un promp una variable se usa $${   }
      const servicios = `Indique cuanto quiere pagar:
      1 - Pago total $${total}
      2 - Pago minimo $${minimo}`;

      const idServicio = prompt(servicios);
      if (parseInt(idServicio)) {
        this.usuario.saldo = this.usuario.saldo - idServicio;
      }
    },

    extraerDinero() {
      const txt = prompt("Ingrese dinero a extraer:");
      console.log(txt);
      let extraer = parseInt(txt);
     // parseInt lo convierte a numero entero && parseFloat lo convierte en numero decimal
      if (
        parseInt(extraer) < this.usuario.saldo &&
        parseInt(extraer) <= this.usuario.limite
      ) {
        this.usuario.saldo = this.usuario.saldo - extraer;
      } else {
        alert("No puedes retirar ese monto");
      }
    },
    depositarDinero() {
      const txt = prompt("Ingrese dinero a depositar:");
      let depositar = parseInt(txt);
      if (parseInt(depositar)) {
        this.usuario.saldo = this.usuario.saldo + depositar;
      }
    },
    cambiarLimiteDeExtraccion() {
      const txt = prompt("Cambiar limite:");
      let cambiar = parseInt(txt);
      if (parseInt(cambiar) <= 12000) {
        this.usuario.limite = cambiar;
      } else {
        alert("no es posible cambiarlo");
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.avatar {
  display: inline-block;

  vertical-align: text-bottom;
}

.logo {
  display: inline-block;
  width: 200px;
  height: 50px;

  text-align: left;
  vertical-align: text-bottom;
}

.align-left {
  display: flex;
  padding: 8px;

  align-items: center;
}

.tu-cuenta {
  padding-left: 24px;
  margin: 0;
  margin-left: -30px;

  font-family: "Source Sans Pro";
  font-size: 32px;
  font-weight: normal;

  color: #2a333d;
  border-left: 6px solid #2cc197;
}

.header {
  display: flex;
  width: 65%;
  margin: 7% auto 0;
  margin-bottom: 15px;

  justify-content: space-between;
}

.cuenta-info {
  display: block;
  width: 100%;
}

#nombre {
  display: inline-block;
  margin-right: 10px;
  overflow: hidden;

  font-size: 14px;

  text-overflow: ellipsis;

  color: #fff;
}

.links {
  position: relative;

  display: block;
  padding: 0;
  margin: 20px 0;
  margin-bottom: 30px;

  font-family: "Source Sans Pro";
  font-size: 16px;

  cursor: pointer;
  -webkit-transition: all 0.6s ease-in-out;
  -moz-transition: all 0.6s ease-in-out;
  -o-transition: all 0.6s ease-in-out;
  transition: all 0.6s ease-in-out;
  text-align: left;
  vertical-align: text-bottom;

  color: #666;
  border: 0;
  background-color: transparent;
}

.links:hover {
  margin-left: 10px;

  color: #2cc197;
}

.white-container {
  display: flex;
  width: 65%;
  padding: 30px;
  margin: 0 auto;
  box-sizing: border-box;

  border: 1px solid #979797;
  border-radius: 8px;
  background-color: #fff;

  justify-content: center;
}

.green-container {
  display: flex;
  width: 65%;
  box-sizing: border-box;

  text-align: center;

  color: white;
  border-radius: 8px;
  background-color: #2cc197;

  align-items: center;
}

.menu-container {
  display: block;
  width: 35%;
  padding-right: 20px;
}

.cuenta-container {
  width: 35%;
}

#saldo-cuenta {
  margin-top: 5%;
  margin-bottom: 5%;
  overflow: hidden;

  font-size: 6.5vw;

  text-align: center;
  text-overflow: ellipsis;
}

#limite-extraccion {
  overflow: hidden;

  font-size: 15px;

  text-overflow: ellipsis;
}

@media only screen and (max-width: 800px) {
  .white-container {
    flex-direction: column-reverse;
  }

  .green-container {
    width: 100%;
    margin-bottom: 20px;
  }

  #saldo-cuenta {
    font-size: 8.5vw;
  }

  .menu-container {
    width: 100%;
  }

  .header {
    margin-bottom: 10px;
    flex-direction: column;

    align-items: center;
  }

  .logo {
    margin-bottom: 10px;
  }
}
</style>
