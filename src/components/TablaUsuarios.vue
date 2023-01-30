<template>
  <div id="tabla-usuarios">
    <div v-if="!usuarios.length" class="alert alert-info" role="alert">
      No existen usuarios.
    </div>
    <table class="table">
      <thead>
      <tr>
        <th>ID</th>
        <th>Nombre de Usuario</th>
        <th>Nombre</th>
        <th>Email</th>
        <th>Ciudad</th>
        <th></th>
        <th></th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="usuario in usuarios" :key="usuario.id">
        <td v-if="editando === usuario.id">
          <label>{{usuario.id}}</label>
        </td>
        <td v-else>{{ usuario.id }}</td>
        <td v-if="editando === usuario.id">
          <input type="text" class="form-control" v-model="usuario.username">
        </td>
        <td v-else>{{ usuario.username }}</td>
        <td v-if="editando === usuario.id">
          <input type="text" class="form-control" v-model="usuario.name">
        </td>
        <td v-else>{{usuario.name}}</td>
        <td v-if="editando === usuario.id">
          <input type="text" class="form-control" v-model="usuario.email">
        </td>
        <td v-else>{{usuario.email}}</td>
        <td v-if="editando === usuario.id">
          <input type="text" class="form-control" v-model="usuario.city">
        </td>
        <td v-else>{{usuario.city}}</td>
        <td v-if="editando ===usuario.id">
          <button class="btn btn-success btn-sm" @click="actualizarusuario(usuario)">💾Guardar</button>
          <button class="btn btn-secondary btn-sm" @click="cancelaredicion(usuario)">❌Cancelar</button>
        </td>
        <td v-else>
          <button class="btn btn-primary btn-sm" @click="editarusuario(usuario)">✏Editar</button>
          <button class="btn btn-danger btn-sm" @click="$emit('eliminarusuario', usuario)">🗑️Eliminar</button>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "tabla-usuarios",
  props: {
    usuarios: Array,
  },
  data(){
    return {
      editando: null,
    }
  },
  methods: {
    editarusuario(usuario) {
      this.usuarioEditado = Object.assign({}, usuario);
      this.editando = usuario.id;
    },
    cancelaredicion(usuario){
      Object.assign(usuario, this.usuarioEditado);
      this.editando = null;
    },
    actualizarusuario(usuario){
      if (!usuario.name.length || !usuario.username.length || !usuario.city.length || !usuario.email.length) {
        return;
      }
      this.$emit('actualizarusuario', usuario);
      this.editando= null;
    }
  }
}
</script>

<style scoped>

</style>