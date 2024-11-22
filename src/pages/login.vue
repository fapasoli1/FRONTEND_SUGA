<script setup>
import authV1MaskDark from '@images/pages/auth-v1-mask-dark.png'
import authV1MaskLight from '@images/pages/auth-v1-mask-light.png'
import logo1 from '@images/logo1.png'
import { useTheme } from 'vuetify'
const form = ref({
  email: '',
  password: '',
  remember: false,
})

const vuetifyTheme = useTheme()

const authThemeMask = computed(() => {
  return vuetifyTheme.global.name.value === 'light' ? authV1MaskLight : authV1MaskDark
})

const isPasswordVisible = ref(false)
</script>

<template>
  <div class="auth-wrapper d-flex align-center justify-center pa-4">
    <VCard class="auth-card pa-4 pt-7 custom-card" max-width="448">
      <VCardItem class="justify-center">
        <div class="d-flex">
          <img :src="logo1" alt="Logo" width="100" />
        </div>
      </VCardItem>

      <VCardText class="pt-2 d-flex justify-center align-center text-center">
        <div>
          <h4 class="text-h4 mb-1 custom-title">Bienvenido a SUGA</h4>
          <p class="mb-0">Inicia sesión en tu cuenta</p>
        </div>
      </VCardText>

      <VCardText>
        <v-form ref="form" validate-on="login lazy" @submit.prevent="login">
          <VRow>
            <!-- email -->
            <VCol cols="12">
              <v-text-field v-model="email" :rules="emailRules" label="Email" placeholder="example@example.com"
                autocomplete="email" />
            </VCol>

            <!-- password -->
            <VCol cols="12">
              <v-text-field v-model="password" label="Contraseña" :rules="passwordRules" placeholder="············"
                :type="isPasswordVisible ? 'text' : 'password'"
                :append-inner-icon="isPasswordVisible ? 'ri-eye-off-line' : 'ri-eye-line'"
                @click:append-inner="isPasswordVisible = !isPasswordVisible" />
            </VCol>

            <!-- remember me checkbox -->
            <div class="d-flex align-center justify-space-between flex-wrap my-6" style="gap: 110px;">
              <VCheckbox v-model="form.remember" label="Recordar" />

              <a href="javascript:void(0)" style="color: #0090a5;">
                Olvidaste la contraseña?
              </a>
            </div>

            <!-- login button -->
            <VCol cols="12">
              <VBtn block type="submit" color="#02b002" class="my-button">
                Login
              </VBtn>
            </VCol>
          </VRow>
        </v-form>
      </VCardText>
    </VCard>
  </div>
</template>


<script>
import axios from 'axios'

export default {
  data: () => ({
    API: process.env.VUE_APP_API,
    loading: false,
    email: '',
    emailRules: [
      value => !!value || 'E-mail is required.',
      value => /.+@.+\..+/.test(value) || 'E-mail must be valid.',
    ],
    password: '',
    passwordRules: [
      value => !!value || 'Password is required.',
      
    ],
  }),
  methods: {
    async login() {
      const isValid = this.$refs.form.validate() 

      if (!isValid) {
        console.log('Formulario no válido')
        return 
      }

      try {
        const response = await axios.post(`http://localhost:3000/auth/login`, {
          email: this.email,
          password: this.password,
        })

        console.log(response)
        this.$store.commit('setUser', response.data)

        this.$store.dispatch('login')
        this.$router.push({ path: '/sugas' })
      } catch (error) {
        if (error.response) {
          console.error('Error de respuesta:', error.response.data)
          console.error('Código de estado:', error.response.status)
          console.error('Encabezados:', error.response.headers)
        } else if (error.request) {
          console.error('Sin respuesta del servidor:', error.request)
        } else {
          console.error('Error en la solicitud:', error.message)
        }
        console.error('Configuración completa del error:', error.config)
      }
    },
  },
}
</script>

<style scoped>

.auth-card {
  background-color: #E6E6E6;
  border-radius: 30px; 
  box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.2); 
}

.my-button:hover {
  background-color: #45a049;
}

.custom-title {
  color: #01a6bd;
}
</style>
