<template>
    <div>
        <HeaderView />
        <div class="container">
            <div class="titulo">
                <h1>Mis restaurantes</h1>
                <div class="line"></div>
            </div>
            <div class="superior">
                <router-link to="/formulario">
                    <button>Agregar restaurante</button>
                </router-link>
            </div>
            <div class="restaurant-list">
                <div v-if="restaurante.lista && restaurante.lista.length > 0">
                    <div class="restaurant-card" v-for="restaurante in restaurante.lista" :key="restaurante.id">
                        <div class="restaurant-image">
                            <img :src="getRestaurantImageUrl(restaurante.logo)" alt="Imagen del restaurante">
                        </div>
                        <div class="restaurant-info">
                            <h2>{{ restaurante.nombreRestaurante }}</h2>
                            <p>{{ restaurante.descripcion }}</p>
                        </div>
                        <div class="restaurant-actions">
                            <router-link :to="{ name: 'update', params: { id: restaurante.id } }">
                                <i class="fa-solid fa-pen-to-square" style="color: #000000;"></i>
                            </router-link>
                            <router-link to="/dashboard">
                                <i class="fa-solid fa-house" style="color: #000000;"></i>
                            </router-link>
                        </div>
                    </div>
                </div>
                <div v-else>
                    <p class="error">No hay restaurantes disponibles.</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import HeaderView from '@/components/header/HeaderView.vue';
import axios from 'axios';

export default {
    name: 'App',
    components: {
        HeaderView
    },
    data() {
        return {
            restaurante: {
                lista: [],
                form: {
                    id: null,
                    nombreRestaurante: '',
                    ubicacion: '',
                    tipoComida: '',
                    objetivos: '',
                    descripcion: ''
                }
            }
        };
    },
    created() {
        this.getRestaurantes();
    },
    methods: {
        async getRestaurantes() {
            try {
                const response = await axios.get("http://localhost:4200/restaurante");
                this.restaurante.lista = response.data;
            } catch (err) {
                console.log(err);
            }
        },
        getRestaurantImageUrl(imageName) {
            if (imageName) {
                return `http://localhost:4200/img/usuario/${imageName}`;
            } else {
                return '../../assets/img/entrada.png'; // Ruta de una imagen predeterminada si no hay logo
            }
        }
    }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');

* {
    font-family: "Poppins", sans-serif;
    font-weight: 400;
    font-style: normal;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
.error{
    margin-left: 160px;
    margin-top: 80px;
    line-height: 1.5;
}
.titulo{
    position: absolute;
    top: 120px;
    margin-left: -140px;
    font-family: "Poppins", sans-serif;
}
.line{
    border: none;
    height: 2px; 
    background-color: #000;
    position: absolute;
    top: 50px; 
    width: 350%;
}
.superior {
    display: block;
    margin-top: 210px;
    margin-left: 290px;
    width: 90%;
    height: 10%;
}
.superior button {
    background-color: #FF7A00;
    border: none;
    border-radius: 20px;
    height: 30px;
    padding: 0 60px;
    margin-left: 740px;
    color: white;
    white-space: nowrap; /* Evita que el texto se divida en varias líneas */
    overflow: hidden; /* Opcional: oculta cualquier texto que se desborde */
    text-overflow: ellipsis; /* Opcional: agrega puntos suspensivos si el texto se desborda */
}
.restaurant-card {
    width: 300px;
    position: absolute;
    left: 140px;
    top: -40px;
    border-radius: 10px;
    overflow: hidden;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    margin: 20px;
    position: relative;
}

.restaurant-image {
    position: relative;
}

.restaurant-image img {
    width: 100%;
    height: 300px;
    display: block;
}

.restaurant-info {
    position: absolute;
    bottom: 10px;
    left: 10px;
    color: white;
}

.restaurant-info h2 {
    margin: 0;
    position: absolute;
    top: -110px;
    left: 70px;
    color: #ffffff;
    font-size: 25px;
}

.restaurant-info p {
    margin: 0;
    position: relative;
    display: flex;
    font-size: 18px;
    color: #ffffff;
    padding: 10px;
    line-height: 1.5;
}

.restaurant-actions {
    position: absolute;
    top: 10px;
    right: 10px;
    display: flex;
    gap: 10px;
}
.restaurant-actions i {
    font-size: 24px;
}
.restaurant-actions button {
    background-color: #FF7A00;
    border: none;
    border-radius: 50%;
    width: 30px;
    height: 30px;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
}

/* Media Query for screens 720px or smaller */
@media (max-width: 720px) {
  .error {
    margin-left: 50px;
    margin-top: 40px;
    font-size: 14px;
  }
  .titulo {
    top: 100px;
    left: 190px;
    font-size: 20px;
  }

  .line {
    top: 66px;
    width: 180%;
  }

  .superior {
    margin-top: 190px;
    margin-left: 420px;
    width: 40%;
    height: auto;
  }

  .superior button {
    margin-left: 0;
    width: 100%;
    padding: 10px ;
  }

  .restaurant-card {
    width: 300px;
    left: 40px;
    top: 0;
    margin: 20px 0;
    box-shadow: none;
  }

  .restaurant-image img {
    height: 140px;
    width: 300px;
  }

  .restaurant-info h2 {
    font-size: 20px;
    top: -50px;
    left: 10px;
  }

  .restaurant-info p {
    font-size: 16px;
    padding: 5px;
  }

  .restaurant-actions {
    top: 5px;
    right: 5px;
    gap: 5px;
  }

  .restaurant-actions button {
    width: 25px;
    height: 25px;
    font-size: 18px;
  }
}
</style>