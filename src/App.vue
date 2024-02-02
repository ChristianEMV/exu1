<template>

  <div class="container">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div>
      <a>Bienvenido a Auto-Marketplace</a>
    </div>
    <div class="card">
      <div class="card-header">
        <h3>Formulario de Registro</h3>
      </div>
      <div class="card-body">

  <!--Body Formulario -->
    <form @submit="submitForm" v-if="showForm">

      <!--Area de marca-->
      <b-form-group label="Marca" label-for="marca">
        <b-form-input
          id="marca"
          v-model="formulario.marca"
          required
        ></b-form-input>
      </b-form-group>
      <br>

      <!--Area año de fabricación-->
      <b-form-group label="Año de fabricación" label-for="anoFabricacion">
        <b-form-input
          type="date"
          id="anoFabricacion"
          v-model="formulario.anoFabricacion"
          :max="maxAnoFabricacion"
          required
        ></b-form-input>
      </b-form-group>
      <br>


      <!--Area número de serie-->
      <b-form-group label="Número de serie" label-for="numserie">
        <b-form-input
          id="numserie"
          v-model="formulario.numserie"
          required
        ></b-form-input>
      </b-form-group>
      <br>
      <!-- Boton enviar --> 
      <b-button type="submit" variant="primary">Enviar formulario</b-button>
    </form>

    <!--Autos registrados-->
    <div v-else>
      <h2>¡Auto registrado!</h2>
      <p>Marca: {{ formulario.marca }}</p>
      <p>Año de fabricación: {{ formulario.anoFabricacion }}</p>
      <p>Número de serie: {{ formulario.numserie }}</p>
      <div>
          <b-table striped hover :items="items"></b-table>
      </div>
    </div>
      </div>
    </div>

  </div>
</template>

<script>
import {
  BForm,
  BFormGroup,
  BFormInput,
  BFormFile,
  BButton,
} from "bootstrap-vue";

export default {
  data() {
      return {
        items: [
          { Marca: {marca}, Año_Fabricacion: {anoFabricacion}, Num_Serie: {numserie} }
        ]
      }
    },
  components: {
    BForm,
    BFormGroup,
    BFormInput,
    BFormFile,
    BButton,
  },
  data() {
    return {
      formulario: {
        marca: "",
        anoFabricacion: "",
        numserie: "",
      },
      showForm: true,
    };
  },
  computed: {
    marca() {
      return `${this.formulario.marca}`.trim();
    },

    maxAnoFabricacion() {
      const currentDate = new Date();
      const maxDate = new Date(
        currentDate.getFullYear(),
        currentDate.getMonth(),
        currentDate.getDate()
      );
      return maxDate.toISOString().split("T")[0];
    },
  },
  methods: {
    submitForm() {
      if (this.validarFormulario()) {

        // Validar número de serie
        if (this.formulario.numserie.length !== 12) {
          alert("El número de serie debe tener exactamente 11 dígitos.");
          return;
        }

        //Validar Año de fabricación
        const anoFabricacion = new Date(this.formulario.anoFabricacion);
        const anio = Math.floor(
          (Date.now() - anoFabricacion.getTime())
        );
        if (anio < 31) {
          alert("La fecha no puede ser mayor al día de registro.");
          return;
        }

        console.log(this.formulario);
        this.showForm = false;
      }
    },
    validarFormulario() {
      if (
        !this.formulario.marca ||
        !this.formulario.anoFabricacion ||
        !this.formulario.numserie
      ) {
        alert("Por favor, completa el formulario.");
        return false;
      }

      return true;
    },
  },
};


</script>
<style scoped>
.container {
    max-width: 500px;
    margin: 0 auto;
    padding: 10px;
  }
  
  .card {
    border: 1px solid #ccc;
    border-radius: 8px;
    box-shadow: 0 10px 10px rgba(0, 0, 0, 0.1);
  }
  
  .card-header {
    color:#ffff;
    padding: 10px;
    background-color: #2a63ff;
    border-bottom: 1px solid #ccc;
  }
  
  .card-body {
    padding: 20px;
  }
</style>