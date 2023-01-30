<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-md-12 nt-2">
        <h3>GESTIÓN CLIENTES</h3>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <formulario-usuario @alta-usuario='postUsuario'></formulario-usuario>
        <tabla-usuarios :usuarios="usuarios" @eliminarusuario="deleteUsuario" @editar-usuario="editarusuario"
                        @cancelar-edicion="cancelaredicion" @actualizarusuario="putusuario"></tabla-usuarios>
      </div>
    </div>
  </div>
</template>

<script>
//para que funcione hay que darle a esto :  json-server --watch .\clients.json
import TablaUsuarios from "@/components/TablaUsuarios.vue";
import FormularioUsuario from "@/components/FormularioUsuario.vue";

export default {
  name: 'App',
  components: {
    FormularioUsuario,
    TablaUsuarios,
  },
  data() {
    return {
      usuarios: [],
    }
  },
  methods: {
    async postUsuario(usuario) {
      try {
        const response = await fetch('http://localhost:3000/usuarios', {
          method: 'POST',
          body: JSON.stringify(usuario),
          headers: {'Content-type': 'application/json; charser=UTF-8'}
        });
        const usuarioAlta = await response.json();
        this.usuarios=[...this.usuarios, usuarioAlta];
      } catch (error) {
        console.error(error);
      }
    },
    async getUsuarios() {
      try {
        const response = await fetch('http://localhost:3000/usuarios');
        this.usuarios = await response.json();
      } catch (error) {
        console.log("Error en get usuarios: ", error);
      }
    },
    async deleteUsuario(usuario) {
      try {
        await fetch(`http://localhost:3000/usuarios/${usuario.id}`, {
          method: 'DELETE'
        });
        this.usuarios = this.usuarios.filter(u => u.id !== usuario.id);
      } catch (error) {
        console.error("Error en borrar usuario: ", error);
      }
    },
    async putusuario(usuario) {
      try {
        const response = await fetch(`http://localhost:3000/usuarios/${usuario.id}`, {
          method: 'PUT',
          body: JSON.stringify(usuario),
          headers: {'Content-type': 'application/json; charset=UTF-8'},
        });
        const usuarioactualizado = await response.json();
        this.usuarios = this.usuarios.map(u => (u.id === usuario.id ? usuarioactualizado : u));
      } catch (error) {
        console.error(error);
      }
    }
  },
  mounted() {
    this.getUsuarios();
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
