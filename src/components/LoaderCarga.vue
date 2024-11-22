<template>
  <v-dialog v-model="estado" max-width="320" persistent>
    <v-list class="py-2" color="primary" elevation="12" rounded="lg">
      <v-list-item prepend-icon="$vuetify-outline" title="Procesando ...">
        <template v-slot:prepend>
          <div class="pe-4">
            <!-- Usando la ruta relativa correcta desde la raíz -->
            <img src="/logo1.png" alt="Logo de Sugaas" width="40" height="40">
          </div>
        </template>

        <template v-slot:append>
          <v-progress-circular
            color="primary"
            indeterminate="disable-shrink"
            size="16"
            width="2"
          ></v-progress-circular>
        </template>
      </v-list-item>
    </v-list>
  </v-dialog>
</template>

<script>
export default {
  props: {
    dialog: {
      type: Boolean,
      required: false,
    },
  },
  data() {
    return {
      estado: false,
    };
  },

  watch: {
    dialog(newValue) {
      this.estado = this.dialog;
    },
  },
};
</script>
