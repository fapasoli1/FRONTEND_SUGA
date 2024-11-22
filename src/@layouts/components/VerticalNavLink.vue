<script setup>
const props = defineProps({
  item: {
    type: null,
    required: true,
  },
})
</script>

<template>
  <li
    class="nav-link"
    :class="{ disabled: item.disable }"
  >
    <Component
      :is="item.to ? 'RouterLink' : 'a'"
      :to="item.to"
      :href="item.href"
      :target="item.target"
    >
      <VIcon
        :icon="item.icon || 'ri-checkbox-blank-circle-line'"
        class="nav-item-icon"
      />
      <!-- 👉 Title -->
      <span
        class="nav-item-title"
        style="color: white"
      >
        {{ item.title }}
      </span>
      <span
        class="nav-item-badge"
        :class="item.badgeClass"
      >
        {{ item.badgeContent }}
      </span>
    </Component>
  </li>
</template>

<style lang="scss">
.layout-vertical-nav {
  .nav-link a {
    display: flex;
    align-items: center;
    cursor: pointer;
    color: #ffffff !important; /* Cambia el color predeterminado del enlace a blanco */
    text-decoration: none; /* Elimina el subrayado del enlace (opcional) */
  }

  /* Cambiar el color de los enlaces cuando se visitan */
  .nav-link a:visited {
    color: #04324D!important; /* Cambia el color de los enlaces visitados a blanco */
  }
  .nav-link a:focus,
.nav-link a:active {
  outline: none; /* Elimina el contorno predeterminado */
  background-color: #04324D; /* Cambia el color de fondo cuando se hace clic */
  color: #ffffff; /* Cambia el color del texto */
  border: 2px solid #04324D; /* Cambia el borde para simular el "cuadro" */
}
}
</style>