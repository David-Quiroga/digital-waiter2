<template>
  <HeaderView/>
  <!-- Contenido principal -->
  <main>
    <div class="hero">
      <h1 class="titulo">{{ isEditing ? 'Editar Restaurante' : 'Creación de Restaurante' }}</h1>
    </div>

    <!-- Formulario de información del restaurante -->
    <div class="formulario">
      <div>
        <h4>Nombre del restaurante</h4>
        <input class="boton1" v-model="nombreRestaurante">
      </div>

      <div class="nombre2">
        <h4>Ubicacion del restaurante</h4>
        <input class="boton2" v-model="ubicacion">
      </div>

      <div class="nombre3">
        <h4>Tipo de comida</h4>
        <input class="boton3" id="comida-select" v-model="tipoComida">
      </div>

      <div class="nombre4">
        <h4>Descripcion del negocio</h4>
        <input class="boton4" v-model="descripcion">
      </div>
    </div>

    <!-- Botones de navegación -->
    <div class="botones">
      <router-link to="/restaurante">
        <button class="btn-back">← Atras</button>
      </router-link>
      
      <button class="btn-conf" @click="submitForm">Continuar</button>
    </div>
  </main>

  <div class="izq3">
    <label class="imagen" for="imagen">Imagen del Negocio:</label>
    <input type="file" class="logo" name="imagen" @change="onImageChange">
    <h4 class="mision">Mision y Vision (opcional)</h4>
    <textarea class="iz1" v-model="objetivos"></textarea>
  </div>
</template>

<script>
import HeaderView from '@/components/header/HeaderView.vue';
import axios from 'axios';
import Swal from 'sweetalert2';

export default {
  name: 'FormularioView',
  components: {
    HeaderView
  },
  data() {
    return {
      nombreRestaurante: "",
      ubicacion: "",
      objetivos: "",
      descripcion: "",
      tipoComida: "",
      logo: null,
      isEditing: false
    };
  },
  props: ['id'],
  created() {
    if (this.id) {
      this.isEditing = true;
      this.loadRestaurante();
    }
  },
  methods: {
    async loadRestaurante() {
      try {
        const response = await axios.get(`http://localhost:4200/restaurante/${this.id}`);
        const restaurante = response.data;
        this.nombreRestaurante = restaurante.nombreRestaurante;
        this.ubicacion = restaurante.ubicacion;
        this.objetivos = restaurante.objetivos;
        this.descripcion = restaurante.descripcion;
        this.tipoComida = restaurante.tipoComida;
      } catch (error) {
        console.error('Error cargando el restaurante:', error);
      }
    },
    async submitForm() {
      // Validación de campos obligatorios
      if (!this.nombreRestaurante || !this.ubicacion || !this.tipoComida || !this.objetivos || !this.descripcion) {
        Swal.fire({
          icon: 'warning',
          title: 'Campos obligatorios',
          text: 'Por favor, completa todos los campos antes de enviar.',
          confirmButtonText: 'Entendido'
        });
        return; // Detener la ejecución si falta algún campo
      }

      try {
        const formData = new FormData();
        formData.append('nombreRestaurante', this.nombreRestaurante);
        formData.append('ubicacion', this.ubicacion);
        formData.append('tipoComida', this.tipoComida);
        formData.append('objetivos', this.objetivos);
        formData.append('descripcion', this.descripcion);
        if (this.logo) {
          formData.append('logo', this.logo);
        }

        if (this.isEditing) {
          await axios.put(`http://localhost:4200/restaurante/${this.id}`, formData, {
            headers: {
              "Content-Type": "multipart/form-data"
            }
          });
          Swal.fire({
            icon: 'success',
            title: 'Restaurante actualizado',
            text: 'El restaurante ha sido actualizado con éxito.',
            confirmButtonText: 'Aceptar'
          });
        } else {
          await axios.post("http://localhost:4200/restaurante", formData, {
            headers: {
              "Content-Type": "multipart/form-data"
            }
          });
          Swal.fire({
            icon: 'success',
            title: 'Restaurante creado',
            text: 'El restaurante ha sido creado con éxito.',
            confirmButtonText: 'Aceptar'
          });
        }

        // Limpieza de los datos del formulario
        this.nombreRestaurante = "";
        this.ubicacion = "";
        this.tipoComida = "";
        this.objetivos = "";
        this.descripcion = "";
        this.logo = null;

        this.$router.push("/restaurante");
      } catch (err) {
        console.error('Error al enviar el formulario:', err);
      }
    },
    onImageChange(event) {
      this.logo = event.target.files[0];
    }
  }
}
</script>

<style scoped>
/* Estilos generales */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #141313;
}

/* Estilos para el contenido principal */
.main {
  padding: 20px;
}

.hero {
  margin-top: 240px;
}

.titulo {
  background-color: #00000000;
  width: 350px;
  box-shadow: none;
  position: absolute;
  left: 100px;
  top: 140px;
  white-space: nowrap;
}

h4 {
  color: #000;
}

.formulario {
  margin-top: 50px;
  margin-left: 80px;
  color: #6c6c6c;
  font-family: 'Montserrat', sans-serif;
}

.izquierda, .derecha {
  display: flex;
  flex-direction: column;
  width: 47%;
}

input {
  background-color: #d3d1d1;
  border: 1px solid #000; /* Borde negro */
  border-radius: 10px;
  height: 40px;
  margin-top: 10px;
  margin-bottom: 21px;
  padding-left: 10px; /* Espacio a la izquierda del texto */
  padding-right: 10px; /* Espacio a la derecha del texto */
}

textarea {
  height: 150px;
}

.mision {
  margin-top: 95px;
}

.imagen {
  position: absolute;
  margin-top: 4px;
}

.logo {
  position: absolute;
  top: 30px;
}

.botones {
  display: flex;
  justify-content: space-between;
  margin-top: 40px;
  margin-left: 100px;
}

.btn-back, .btn-conf {
  height: 40px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  margin-right: 10px;
}

.btn-back {
  background-color: #BBB7B7;
  width: 250px;
  margin-left: -18px;
}

.btn-conf {
  background-color: #FF7A00;
  color: #FFFFFF;
  width: 250px;
  position: absolute;
  margin-left: 260px;
}

.atras1 {
  background-color: #BBB7B7;
  border: none;
  color: #000000;
}

.continuar {
  background-color: #ff7a00;
  border: none;
  color: #000;
}

.izq1 {
  width: 100px;
  height: 30px;
  margin-left: -580px; 
  margin-top: -10px;
  margin-bottom: 30px;
  text-align: left; 
  line-height: 30px; 
  position: relative;
  white-space: nowrap;
}

.izq3 {
  width: 100px;
  height: 30px;
  margin-left: -600px; 
  margin-top: -204px;
  margin-bottom: 30px;
  color: #000000;
  text-align: left; 
  line-height: 30px; 
  position: relative;
  white-space: nowrap;
}

/* Media Query para pantallas de 720px o menos */
@media (max-width: 720px) {
  .titulo {
    left: 20px;
    top: 100px;
    width: auto;
    font-size: 18px;
  }
  input{
    position: relative;
    top: -20px;
    width: 100;
    height: 30px;
    border-radius: 5px;
  }
  label{
    position: absolute;
    top: 10%;
  }
  h4{
    position: relative;
    top: -16px;
    white-space: nowrap;
  }
  .izquierda, .derecha {
    width: 100%;
  }
  .nombre3{
    position: relative;
    top: -20px;
  }
  .nombre4{
    position: relative;
    top: -30px;
  }
  .botones {
    flex-direction:row;
  }
  .boton2{
    top: -20px;
  }
  .btn-back, .btn-conf {
    top: 100%;
    margin: 10px 0;
  }

  .btn-conf {
    position: relative;
    margin-top: 20%;
    left: 90;
  }
  .btn-back {
    position: relative;
    top: 60%;
    left: -30px;
  }
  .izq3{
    position: absolute;
    top: 634px;
    left: 680px;
    color: #000;
  }
  .hero {
    margin-top: 150px;
  }
  .imagen{
    white-space: nowrap;
  }
  .mision {
    margin-top: 95px;
  }
  textarea {
  margin-top: -10px;
  height: 50px;
  }
}
</style>