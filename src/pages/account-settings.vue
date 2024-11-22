<script setup>
import authV1MaskDark from '@images/pages/auth-v1-mask-dark.png'
import authV1MaskLight from '@images/pages/auth-v1-mask-light.png'
import { useTheme } from 'vuetify'

const vuetifyTheme = useTheme()

const authThemeMask = computed(() => {
  return vuetifyTheme.global.name.value === 'light' ? authV1MaskLight : authV1MaskDark
})

const isPasswordVisible = ref(false)
</script>

<template>
  <div class="auth-wrapper d-flex align-center justify-center pa-4">
    <VCard
      class="auth-card pa-5"
      max-width="500"
      elevation="3"
    >
      <VCardItem class="justify-center mb-6">
        <RouterLink
          to="/"
          class="d-flex align-center gap-3"
        />
      </VCardItem>

      <VCardText>
        <h4 class="text-h4 mb-3" style="color: #01a6bd;">
          Datos de tu cuenta
        </h4>
        <VForm
          ref="form"
          @submit.prevent="() => {}"
        >
          <VRow>
            <VCol cols="12">
              <h5 class="label">
                Nombre:
              </h5>
              <p class="user-info">
                {{ name }}
              </p>
            </VCol>

            <VCol cols="12">
              <h5 class="label">
                Email:
              </h5>
              <p class="user-info">
                {{ email }}
              </p>
            </VCol>

            <VCol cols="12">
              <h5 class="label">
                Cédula:
              </h5>
              <p class="user-info">
                {{ cedula }}
              </p>
            </VCol>

            <VCol cols="12">
              <h5 class="label">
                Teléfono:
              </h5>
              <p class="user-info">
                {{ telefono }}
              </p>
            </VCol>

            <VCol cols="12">
              <h5 class="label">
                Rol:
              </h5>
              <p class="user-info">
                {{ role }}
              </p>
            </VCol>
          </VRow>
        </VForm>
      </VCardText>
    </VCard>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      valid: false,
      name: this.$store.getters.getUser.nombre,
      email: this.$store.getters.getUser.email,
      cedula: this.$store.getters.getUser.cedula,
      telefono: this.$store.getters.getUser.telefono,
      role: this.$store.getters.getUser.rol,
      id: null,
      dialog: false,
    }
  },

  methods: {
    async CambiarContrasena() {
      try {
        const response = await axios.post(
          'http://localhost:3000/usuarios/recuperar-contrasena',
          { email: this.email },
          {
            headers: {
              Authorization: `Bearer ${this.$store.getters.getUser.access_token}`,
            },
          },
        )

        this.$notify({ text: response.data.message, type: 'success' }) // Cambia el tipo según sea necesario;
      } catch (error) {
        console.error('Error al obtener los roles:', error)
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.auth-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f8f9fa;
  min-block-size: 100vh;
}

.auth-card {
  border-radius: 12px;
  background-color:#E6E6E6;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 10%);
}

.title {
  color: #2d882d;
  font-weight: 600;
}

h5.label {
  color: #080808;
  font-weight: bold;
  margin-block-end: 0.5rem;
}

p.user-info {
  color: #2c2a2a;
  font-size: 1.1rem;
  margin-block-end: 1.5rem;
}

.logo-container {
  inline-size: 50px;
}

.btn-submit {
  background-color: #2d882d !important;
  color: white !important;
  font-weight: 600;
}

.v-btn:hover {
  background-color: #256c25 !important;
}

.v-btn:active {
  background-color: #1d571d !important;
}
</style>
