<template>
  <card>
    <div slot="header">
      <h4 class="card-title">
        {{ config.selectedDevice.name }} - {{ config.variableFullName }}
      </h4>
    </div>

    <p>Mensaje a Enviar: <h5>{{config.message}} </h5></p>

    <!-- Contenedor flexible para el icono y el select -->
    <div class="icon-select-container">
      <i
        class="fa"
        :class="[config.icon, getIconColorClass()]"
        style="font-size: 30px; margin-right: 10px;"
      ></i>

      <el-select
        v-model="selectedNumber"
        :class="[getSelectColorClass()]"
        placeholder="Selecciona o ingresa un número"
        :disabled="isSelectDisabled"
        filterable
        allow-create
        @change="addNewNumber"
      >
        <el-option
          v-for="option in numbers"
        :class="[ getSelectColorClass()]"
          :value="option.value"
          :label="option.label"
          :key="option.label"
        ></el-option>
      </el-select>
    </div>

    <base-button
      @click="sendValue()"
      :type="config.class"
      class="mb-3 pull-right"
      size="lg"
    >
      {{ config.text }}
    </base-button>
  </card>
</template>

<script>
import { Select, Option } from 'element-ui';

export default {
  props: ["config"],
  components: {
    'el-select': Select,
    'el-option': Option,
  },
  data() {
    return {
      sending: false,
      isSelectDisabled: false, // Variable para controlar si el select está desactivado

      selectedNumber: '', // Variable para almacenar el numero seleccionado
      numbers: [
        { value: 1, label: '1' },
        { value: 2, label: '2' },
        { value: 3, label: '3' },
        { value: 4, label: '4' },
        { value: 5, label: '5' },
        { value: 6, label: '6' },
        { value: 7, label: '7' },
        { value: 8, label: '8' },
        { value: 9, label: '9' },
        { value: 10, label: '10' },
        { value: 11, label: '11' },
        { value: 12, label: '12' },
        { value: 13, label: '13' },
        { value: 14, label: '14' },
        { value: 15, label: '15' },
        { value: 16, label: '16' },
        { value: 17, label: '17' },
        { value: 18, label: '18' },
        { value: 19, label: '19' },
        { value: 20, label: '20' },
        { value: 21, label: '21' },
        { value: 22, label: '22' },
        { value: 23, label: '23' },
        { value: 24, label: '24' },
        { value: 25, label: '25' },
        { value: 26, label: '26' },
        { value: 27, label: '27' },
        { value: 28, label: '28' },
        { value: 29, label: '29' },
        { value: 30, label: '30' },

      ],
    };
  },
  mounted() {
    // Puedes inicializar el valor seleccionado si es necesario
    this.selectedNumber = this.config.defaultNumber || '';

  },
  methods: {
    sendValue() {
      this.sending = true;

      setTimeout(() => {
        this.sending = false;
      }, 500);

      const toSend = {
        topic:
          this.config.userId +
          "/" +
          this.config.selectedDevice.dId +
          "/" +
          this.config.variable +
          "/actdata",
        msg: {
      value: JSON.stringify({ [this.config.message]: this.selectedNumber }), // Convertir el JSON a cadena
        },
      };

      console.log(toSend);
      this.$nuxt.$emit("mqtt-sender", toSend);
    },

    getIconColorClass() {
      if (!this.sending) {
        return "text-dark";
      }

      if (this.config.class == "success") {
        return "text-success";
      }
      if (this.config.class == "primary") {
        return "text-primary";
      }
      if (this.config.class == "warning") {
        return "text-warning";
      }
      if (this.config.class == "danger") {
        return "text-danger";
      }
    },

    getSelectColorClass() {


      if (this.config.class == "success") {
        return "select-success";
      }
      if (this.config.class == "primary") {
        return "select-primary";
      }
      if (this.config.class == "warning") {
        return "select-warning";
      }
      if (this.config.class == "danger") {
        return "select-danger";
      }
    },


  },
};
</script>

<style scoped>
/* Estilos para el contenedor flexible */
.icon-select-container {
  display: flex;
  align-items: center; /* Alinea verticalmente los elementos */
  margin-bottom: 20px; /* Espacio inferior */
}

/* Estilos para el select */
.select-danger {
  flex-grow: 1; /* Ocupa el espacio restante */
}
</style>

