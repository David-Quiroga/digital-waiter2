<template>
  <HeaderView />
  <aside class="sidebar">
    <nav>
      <ul>
        <li>
          <router-link to="/dashboard" class="active sidebar-link">
            <i class="fa-solid fa-chart-simple"></i> <span>Dashboard</span>
          </router-link>
        </li>
        <li>
          <router-link to="/menus" class="active sidebar-link">
            <i class="fa-solid fa-envelope"></i> <span>Menu</span>
          </router-link>
        </li>
        <li>
          <router-link to="/visualizar" class="active sidebar-link">
            <i class="fa-solid fa-table-cells-large"></i> <span>Visualizar</span>
          </router-link>
        </li>
        <li>
          <router-link to="/empleados" class="active sidebar-link">
            <i class="fa-solid fa-person"></i> <span>Empleados</span>
          </router-link>
        </li>
        <li>
          <router-link to="/pagos" class="active sidebar-link">
            <i class="fa-regular fa-credit-card"></i> <span>Metodos de pago</span>
          </router-link>
        </li>
        <li>
          <router-link to="/proveedores" class="active sidebar-link">
            <i class="fa-solid fa-user-group"></i> <span>Proveedores</span>
          </router-link>
        </li>
        <li>
          <router-link to="/inventario" class="active sidebar-link">
            <i class="fa-solid fa-file-invoice-dollar"></i> <span>Inventario</span>
          </router-link>
        </li>
      </ul>
    </nav>
  </aside>
  <!-- ! Termina el SIDEBAR -->
<div class="content">
    <div class="top">
      <h2>Proveedores</h2>
      <div class="superior">
        <router-link to="/create">
          <button>Agregar Proveedor</button>
        </router-link>
      </div>
    </div>
    <div class="table-container">
      <table>
        <thead>
          <tr>
            <th>Nombre</th>
            <th >Contacto</th>
            <th>Email</th>
            <th class="hide-mobile">Dirección</th>
            <th class="hide-mobile">Ciudad</th>
            <th>Producto</th>
            <th>Estado</th>
            <th>Acciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="proveedor in proveedor.lista" :key="proveedor.idProveedores">
            <td>{{ proveedor.nombreProveedor }}</td>
            <td>{{ proveedor.numContacto }}</td>
            <td>{{ proveedor.emailContacto }}</td>
            <td class="hide-mobile">{{ proveedor.direccion }}</td>
            <td class="hide-mobile">{{ proveedor.ciudad }}</td>
            <td>{{ proveedor.tipoProducto }}</td>
            <td>{{ proveedor.estado }}</td>
            <td class="actions">
              <router-link :to="{ name: 'UpdateProveedores', params: { idProveedores: proveedor.idProveedores }}">
                <i class="fa-solid fa-pen-to-square"></i>
              </router-link>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>


<script>
import HeaderView from '@/components/header/HeaderView.vue';
import axios from 'axios';

export default {
  name: 'ProveedoresView',
  components: {
    HeaderView
  },
  data() {
    return {
      proveedor: {
        lista: [],
        form: {
          idProveedores: null,
          nombreProveedor: '',
          numContacto: '',
          emailContacto: '',
          direccion: '',
          ciudad: '',
          estado: '',        // Colocamos "estado" antes de "tipoProducto"
          tipoProducto: ''
        }
      }
    };
  },
  created() {
    this.getProveedores();
  },
  methods: {
    async getProveedores() {
      try {
        const response = await axios.get('http://localhost:4200/proveedores');
        console.log(response.data);
        this.proveedor.lista = response.data;
      } catch (error) {
        console.error('Error al obtener proveedores:', error);
      }
    },
    async deleteProveedor(id) {
      try {
        await axios.delete(`http://localhost:4200/proveedores/${id}`);
        this.proveedor.lista = this.proveedor.lista.filter(proveedor => proveedor.idProveedores !== id);
        console.log('Proveedor eliminado correctamente');
      } catch (error) {
        console.error('Error al eliminar proveedor:', error);
      }
    }
  }
};
</script>



<style scoped>
.imgP{
    width: 50px;
}
.content {
    margin-left: 50px;
    padding: 20px;
    width: calc(100% - 270px);
}
.superior {
    display: flex;
    margin-top: 10px;
    width: 90%;
    height: 10%;
    align-items: center;
    justify-content: space-between; /* Distribuye los elementos horizontalmente */
    padding: 0 30px;
    box-sizing: border-box; /* Incluye el padding en el ancho total */
}
.superior h1{
    font-weight: bold;
    font-size: 50px;
}
.superior button{
    background-color: #FF7A00;
    border: none;
    border-radius: 20px;
    height: 30px;
    padding: 0 60px;
    margin-left: 740px;
    color: white;
    white-space: nowrap;
}
.top { 
    background-color: #75787b;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    margin-top: 100px;
    margin-left: 19px;
    width: 1200px;
    height: 80px;
    justify-content: space-between;
}
.header h2 {
    font-size: 20px;
    }
.table-container {
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    overflow: hidden;
    padding: 20px;
}
table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 10px;
}
th, td {
    padding: 15px;
}
td{
  text-align: center;
}
th {
    background-color: #bebebe;
    color: #333;
    font-weight: bold;
}
tr:nth-child(even) {
    background-color: #f9fafb;
}
tr:hover {
    background-color: #f1f1f1;
}
.actions i {
    margin-right: 10px;
    cursor: pointer;
}
.actions i:hover {
    color: #FF0000;
}
.header-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #d1d8dd;
    padding: 20px;
    border-radius: 10px;
    margin-bottom: 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
.header-container h1 {
    margin: 0;
    font-size: 24px;
}
@media (max-width: 720px) {
  .sidebar {
    max-width: 70px;
  }
  .sidebar-link {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 10px 0;
  }
  .sidebar-link span {
    display: none;
  }

  .sidebar-link i {
    font-size: 1.5em;
  }
  .content {
    margin-left: 0;
    padding: 10px;
    width: 150%;
  }
  .top{
    width: 90%;
  }
  .superior h1 {
    font-size: 24px;
  }
  .superior button {
    margin-left: 10px;
    padding: 0 20px;
    font-size: 12px;
  }
  .titulo {
    width: 100%;
    flex-direction: column;
    align-items: flex-start;
    padding: 10px;
  }
  .header h2 {
    font-size: 18px;
  }
  .search-box {
    width: 100%;
    margin-top: 10px;
  }
  table {
    margin-top: -12px;
    width: 98%;
    margin-left: 1px;
  }
  th, td {
    padding: 10px;
    font-size: 12px;
  }
  .header-container {
    flex-direction: column;
    align-items: flex-start;
    padding: 10px;
  }
  .header-container h1 {
    font-size: 18px;
    margin-bottom: 10px;
  }
  .titulo {
    width: 96%;
    height: 10%;
    position: relative;
    left: 10px;
    border-radius: 5px;
  }
  .superior button {
    border-radius: 5px;
    position: relative;
    top: -4px;
    width: 100%;
    left: 160px;
  }
  .hide-mobile {
  display: none;
}
}
</style>
