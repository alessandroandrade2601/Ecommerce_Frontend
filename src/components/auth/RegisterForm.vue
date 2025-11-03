<template>
  <div class="register-container">
    <form class="register-form" @submit.prevent="register">
      <h3>Crear cuenta</h3>

      <label for="firstName">Nombre</label>
      <input id="firstName" v-model="form.firstName" type="text" placeholder="Nombre" required />

      <label for="lastName">Apellido</label>
      <input id="lastName" v-model="form.lastName" type="text" placeholder="Apellido" required />

      <label for="email">Correo electrónico</label>
      <input
        id="email"
        v-model="form.email"
        type="email"
        placeholder="ejemplo@correo.com"
        required
      />

      <label for="dateOfBirth">Fecha de nacimiento</label>
      <input id="dateOfBirth" v-model="form.dateOfBirth" type="date" />

      <label for="password">Contraseña</label>
      <input
        id="password"
        v-model="form.password"
        type="password"
        placeholder="Contraseña"
        required
      />

      <label for="country">País(MAYUSCULAS Y SIN TILDE)</label>
      <input id="country" v-model="form.country" type="text" placeholder="País" />

      <label for="address">Dirección</label>
      <textarea id="address" v-model="form.address" placeholder="Dirección" rows="2"></textarea>

      <label for="type">Tipo de usuario</label>
      <select id="type" v-model="form.type">
        <option value="">Seleccionar tipo</option>
        <option value="U">U</option>
      </select>

      <button type="submit" class="btn-register">Registrar</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'RegisterForm',
  data() {
    return {
      form: {
        firstName: '',
        lastName: '',
        email: '',
        dateOfBirth: '',
        password: '',
        country: '',
        address: '',
        type: '',
      },
    }
  },
  methods: {
    register() {
      console.log('Registrando usuario...')
      console.log(this.form)
      // Validación mínima antes de emitir (puedes ampliarla)
      if (!this.form.firstName || !this.form.lastName || !this.form.email || !this.form.password) {
        // En un app real, mostrar feedback al usuario
        console.warn('Faltan campos obligatorios')
        return
      }
      console.log('Datos de registro:', this.form)
      let endpointURL = '/api/user/registro'
      let userData = { ...this.form }

      this.$api
        .post(endpointURL, userData)
        .then((response) => {
          console.log('Registro exitoso:', response)
          // Mostrar notificación de éxito
          if (this.$q && this.$q.notify) {
            this.$q.notify({
              type: 'positive',
              position: 'top',
              message: 'Registro exitoso. Por favor inicia sesión.',
            })
          }
          // Redirigir a la página de login
          if (this.$router) {
            this.$router.push('/login')
          }
        })
        .catch((error) => {
          console.log('Ocurrió un error al registrar: ' + error)
          this.$q.notify({
            type: 'negative',
            position: 'top',
            message: 'Error al registrar usuario.',
          })
        })
    },
  },
}
</script>

<style scoped>
.register-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px;
  background: transparent; /* no forzamos fondo global */
}

.register-form {
  width: 480px;
  max-width: 100%;
  background-color: rgba(255, 255, 255, 0.06);
  border-radius: 10px;
  padding: 28px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.25);
  backdrop-filter: blur(6px);
  color: #000000;
}

.register-form h3 {
  margin: 0 0 12px 0;
  text-align: center;
  font-size: 22px;
}

.register-form label {
  display: block;
  margin-top: 12px;
  font-size: 14px;
  color: #000000;
}

.register-form input,
.register-form textarea,
.register-form select {
  width: 100%;
  padding: 10px 12px;
  margin-top: 6px;
  border-radius: 6px;
  border: 1px solid rgba(255, 255, 255, 0.08);
  background: rgba(255, 255, 255, 0.03);
  color: #000000;
  font-size: 14px;
}

.register-form textarea {
  resize: vertical;
}

.btn-register {
  width: 100%;
  margin-top: 18px;
  padding: 12px 16px;
  border-radius: 8px;
  border: none;
  background: linear-gradient(90deg, #23a2f6, #1845ad);
  color: #fff;
  font-weight: 600;
  cursor: pointer;
}

.btn-register:active {
  transform: translateY(1px);
}
</style>
