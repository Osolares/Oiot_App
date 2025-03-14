<template>
  <div>
    <!-- FORM ADD DEVICE -->
    <div class="row">
      <card>
        <div slot="header">
          <h4 v-if="deviceEdition" class="card-title">ACTUALIZAR DISPOSITIVO</h4>
          <h4 v-else class="card-title">CREAR NUEVO DISPOSITIVO</h4>

        </div>

        <div class="row">
          <div class="col-4">
            <base-input
              label="Device Name"
              type="text"
              placeholder="Ex: Home, Office..."
              v-model="newDevice.name"
            >
            </base-input>
          </div>

          <div class="col-4">
            <base-input
              label="Device Id"
              type="text"
              placeholder="Ex: 7777-8888"
              v-model="newDevice.dId"
            >
            </base-input>
          </div>
          <div class="col-4" v-if="deviceEdition">
            <base-input
              label="Password"
              type="text"
              placeholder="Ex: 7777-8888"
              v-model="newDevice.password"
            >
            </base-input>
          </div>



          <div class="col-4">
            <slot name="label">
              <label> Template </label>
            </slot>

            <el-select
              v-model="selectedIndexTemplate"
              placeholder="Select Template"
              class="select-primary"
              style="width:100%"
            >
              <el-option
                v-for="(template, index) in templates"
                :key="template._id"
                class="text-dark"
                :value="index"
                :label="template.name"
              ></el-option>
            </el-select>
          </div>
        </div>

        <div v-if="deviceEdition" class="row pull-right">
          <div class="col-12">
            <base-button
              @click="updateDevice(newDevice)"
              type="success"
              class="mb-3"
              size="lg"
              >Actualizar Dispositivo</base-button
            >
          </div>
        </div>

        <div v-else class="row pull-right">
          <div class="col-12">
            <base-button
              @click="createNewDevice()"
              type="primary"
              class="mb-3"
              size="lg"
              >Crear</base-button
            >
          </div>
        </div>
      </card>
    </div>

    <!-- DEVICES TABLE -->
    <div class="row">
      <card>
        <div slot="header">
          <h4 class="card-title">Devices</h4>
        </div>

        <el-table :data="$store.state.devices">
          <el-table-column label="#" min-width="50" align="center">
            <div slot-scope="{ row, $index }">
              {{ $index + 1 }}
            </div>
          </el-table-column>

          <el-table-column prop="name" label="Name"></el-table-column>

          <el-table-column prop="dId" label="Device Id"></el-table-column>

          <el-table-column prop="password" label="Password"></el-table-column>

          <el-table-column
            prop="templateName"
            label="Template"
          ></el-table-column>

          <el-table-column label="Actions">
            <div slot-scope="{ row, $index }">

              <el-tooltip content="Edit" effect="light" :open-delay="300" placement="top">
                <base-button @click="editDevice(row)" type="success" icon size="sm">
                  <i class="fa fa-edit"></i>
                </base-button>
              </el-tooltip>

              <el-tooltip
                content="Saver Status Indicator"
                style="margin-right:10px"
              >
                <i
                  class="fas fa-database "
                  :class="{
                    'text-success': row.saverRule.status,
                    'text-dark': !row.saverRule.status
                  }"
                ></i>
              </el-tooltip>

              <el-tooltip content="Database Saver">
                <base-switch
                  @click="updateSaverRuleStatus(row.saverRule)"
                  :value="row.saverRule.status"
                  type="primary"
                  on-text="On"
                  off-text="Off"
                ></base-switch>
              </el-tooltip>

              <el-tooltip
                content="Delete"
                effect="light"
                :open-delay="300"
                placement="top"
              >
                <base-button
                  type="danger"
                  icon
                  size="sm"
                  class="btn-link"
                  @click="deleteDevice(row)"
                >
                  <i class="tim-icons icon-simple-remove "></i>
                </base-button>
              </el-tooltip>
            </div>
          </el-table-column>
        </el-table>
      </card>
    </div>
  </div>
</template>

<script>
import { Table, TableColumn } from "element-ui";
import { Select, Option } from "element-ui";
import Swal from 'sweetalert2';
import 'sweetalert2/dist/sweetalert2.css'

export default {
  middleware: "authenticated",
  components: {
    [Table.name]: Table,
    [TableColumn.name]: TableColumn,
    [Option.name]: Option,
    [Select.name]: Select
  },
  data() {
    return {
      selectedDevice: {},
      deviceEdition : false,
      templates: [],
      selectedIndexTemplate: null,
      newDevice: {
        name: "",
        dId: "",
        templateId: "",
        templateName: ""
      },
      devicePassword : ""
    };
  },

  watch: {

    selectedDevice: {
      handler(newVal) {
        if (newVal) {
      const templateIndex = this.templates.findIndex(
        (template) => template._id === newVal.templateId
      );

      if (templateIndex !== -1) {
        // Actualizar el índice seleccionado
        this.selectedIndexTemplate = templateIndex;
      } 



          this.newDevice.name = newVal.name
          this.newDevice.dId = newVal.dId
          this.newDevice.templateId = newVal.templateId
          //this.selectedIndexTemplate = 0
          this.newDevice.password = newVal.password

          console.log("Device actualizado:", this.selectedDevice);

          
        }
      },
      deep: true,
      immediate: true, // Para que se ejecute al montar el componente

    },
    
  },



  mounted() {
    
    this.getTemplates();
  },
  methods: {
    updateSaverRuleStatus(rule) {
      
      var ruleCopy = JSON.parse(JSON.stringify(rule));

      ruleCopy.status = !ruleCopy.status;

      const toSend = { 
        rule: ruleCopy 
      };

      const axiosHeaders = {
        headers: {
          token: this.$store.state.auth.token
        }
      };

      this.$axios
        .put("/saver-rule", toSend, axiosHeaders)
        .then(res => {


          if (res.data.status == "success") {

            this.$store.dispatch("getDevices");

                        // Mostrar alerta de éxito
             Swal.fire({
              icon: 'success',
              title: 'EXITO',
              text: 'Saver Rule actualizada correctamente.',
            });

            this.$notify({
              type: "success",
              icon: "tim-icons icon-check-2",
              message: " Device Saver Status Updated"
            });

          }

          return;
        })
        .catch(e => {
          console.log(e);
      // Mostrar alerta de error
       Swal.fire({
        icon: 'error',
        title: 'Error',
        text: 'Ocurrió un error al actualizar el estado de la Saver Rule.',
      });

          this.$notify({
            type: "danger",
            icon: "tim-icons icon-alert-circle-exc",
            message: " Error updating saver rule status"
          });
          return;
        });
    },

    deleteDevice(device) {
      const axiosHeaders = {
        headers: {
          token: this.$store.state.auth.accessToken
        },
        params: {
          dId: device.dId
        }
      };

      this.$axios
        .delete("/device", axiosHeaders)
        .then(res => {
          if (res.data.status == "success") {
                        // Mostrar alerta de éxito
            Swal.fire({
              icon: 'success',
              title: 'EXITO',
              text: 'El dispositivo se ha eliminado correctamente.',
            });

            this.$notify({
              type: "success",
              icon: "tim-icons icon-check-2",
              message: device.name + " deleted!"
            });
          }

          $nuxt.$emit("time-to-get-devices");

          return;
        })
        .catch(e => {
          console.log(e);
            // Mostrar alerta de error
             Swal.fire({
              icon: 'error',
              title: 'Error',
              text: 'Error eliminando el dispositivo ' + device.name,
            });
          
          this.$notify({
            type: "danger",
            icon: "tim-icons icon-alert-circle-exc",
            message: " Error deleting " + device.name
          });
          return;
        });
    },
    //new device
    createNewDevice() {
      if (this.newDevice.name == "") {

        this.$notify({
          type: "warning",
          icon: "tim-icons icon-alert-circle-exc",
          message: " Device Name is Empty :("
        });
        return;
      }

      if (this.newDevice.dId == "") {
        this.$notify({
          type: "warning",
          icon: "tim-icons icon-alert-circle-exc",
          message: " Device ID is Empty :("
        });
        return;
      }

      if (this.selectedIndexTemplate == null) {
        this.$notify({
          type: "warning",
          icon: "tim-icons icon-alert-circle-exc",
          message: " Tempalte must be selected"
        });
        return;
      }

      const axiosHeaders = {
        headers: {
          token: this.$store.state.auth.token
        }
      };

      //ESCRIBIMOS EL NOMBRE Y EL ID DEL TEMPLATE SELECCIONADO EN EL OBJETO newDevice
      this.newDevice.templateId = this.templates[
        this.selectedIndexTemplate
      ]._id;
      this.newDevice.templateName = this.templates[
        this.selectedIndexTemplate
      ].name;

      const toSend = {
        newDevice: this.newDevice
      };

      this.$axios
        .post("/device", toSend, axiosHeaders)
        .then(res => {
          if (res.data.status == "success") {
            this.$store.dispatch("getDevices");

            this.newDevice.name = "";
            this.newDevice.dId = "";
            this.selectedIndexTemplate = null;
                                    // Mostrar alerta de éxito
            Swal.fire({
              icon: 'success',
              title: 'EXITO',
              text: 'El dispositivo se ha agregado correctamente.',
            });

            this.$notify({
              type: "success",
              icon: "tim-icons icon-check-2",
              message: "Success! Device was added"
            });

            return;
          }
        })
        .catch(e => {
          if (
            e.response.data.status == "error" &&
            e.response.data.error.errors.dId.kind == "unique"
          ) {
            // Mostrar alerta de error
             Swal.fire({
              icon: 'error',
              title: 'Error',
              text: 'El dispositivo ya esta registrado en el sistema. Intente con otros datos',
            });

            this.$notify({
              type: "warning",
              icon: "tim-icons icon-alert-circle-exc",
              message:
                "The device is already registered in the system. Try another device"
            });
            return;
          } else {
              // Mostrar alerta de error
             Swal.fire({
              icon: 'error',
              title: 'Error',
              text: 'Error desconocido',
            });
            this.showNotify("danger", "Error");
            return;
          }
        });
    },

    //Get Templates
    async getTemplates() {
      const axiosHeaders = {
        headers: {
          token: this.$store.state.auth.token
        }
      };

      try {
        const res = await this.$axios.get("/template", axiosHeaders);
        console.log(res.data);

        if (res.data.status == "success") {
          this.templates = res.data.data;
        }
      } catch (error) {

                    // Mostrar alerta de error
             Swal.fire({
              icon: 'error',
              title: 'Error',
              text: 'Error obteniendo plantillas',
             });
            
        this.$notify({
          type: "danger",
          icon: "tim-icons icon-alert-circle-exc",
          message: "Error getting templates..."
        });
        console.log(error);
        return;
      }
    },

    deleteDevice(device) {
      const axiosHeader = {
        headers: {
          token: this.$store.state.auth.token
        },
        params: {
          dId: device.dId
        }
      };

      this.$axios
        .delete("/device", axiosHeader)
        .then(res => {
          if (res.data.status == "success") {

                                    // Mostrar alerta de éxito
            Swal.fire({
              icon: 'success',
              title: 'EXITO',
              text: 'El dispositivo se ha eliminado correctamente.' + device.name,
            });

            this.$notify({
              type: "success",
              icon: "tim-icons icon-check-2",
              message: device.name + " deleted!"
            });
            this.$store.dispatch("getDevices");
          }
        })
        .catch(e => {
          console.log(e);

                      // Mostrar alerta de error
             Swal.fire({
              icon: 'error',
              title: 'Error',
              text: 'Error eliminando ' + device.name,
             });
            
          this.$notify({
            type: "danger",
            icon: "tim-icons icon-alert-circle-exc",
            message: " Error deleting " + device.name
          });
        });
    },

        // Editar widget - Carga datos en selectedTemplate y selectedWidget, activa edición
    editDevice(device) {
      this.selectedDevice = { ...device }; // Clonamos el objeto
      this.deviceEdition = true;
         //Hacer scroll al inicio de la página
      window.scrollTo({
        top: 0,
        behavior: "smooth"
      });

    },

          //Update Template
async updateDevice() {
  if (!this.newDevice.name || !this.newDevice.dId || !this.newDevice.password) {
    this.$notify({
      type: "warning",
      icon: "tim-icons icon-alert-circle-exc",
      message: "All fields are required!"
    });
    return;
  }

  if (this.selectedIndexTemplate == null) {
    this.$notify({
      type: "warning",
      icon: "tim-icons icon-alert-circle-exc",
      message: "Template must be selected"
    });
    return;
  }

  const axiosHeaders = {
    headers: { token: this.$store.state.auth.token }
  };

  // Asignar template seleccionado
  this.newDevice.templateId = this.templates[this.selectedIndexTemplate]._id;
  this.newDevice.templateName = this.templates[this.selectedIndexTemplate].name;

  const toSend = { newDevice: this.newDevice };

  try {
    const res = await this.$axios.put("/updatedevice", toSend, axiosHeaders);
    
    if (res.data.status === "success") {
      this.$store.dispatch("getDevices");

      // Reiniciar formulario
      this.newDevice = { name: "", dId: "", password: "" };
      this.selectedIndexTemplate = null;
      this.selectedDevice = {};
      this.deviceEdition = false;
            // Mostrar alerta de éxito
      await Swal.fire({
        icon: 'success',
        title: 'Dispositivo',
        text: 'El dispositivo se ha actualizado correctamente.',
      });
      this.$notify({
        type: "success",
        icon: "tim-icons icon-check-2",
        message: "Success! Device was updated"
      });
    }
  } catch (e) {
    if (e.response && e.response.data.status === "error") {
      // Mostrar alerta de error
      await Swal.fire({
        icon: 'error',
        title: 'Error',
        text: error.message || 'Ocurrió un error al actualizar el dispositivo.',
      });

      this.$notify({
        type: "warning",
        icon: "tim-icons icon-alert-circle-exc",
        message: e.response.data.message || "Error updating device"
      });
    } else {
      // Mostrar alerta de error
      await Swal.fire({
        icon: 'error',
        title: 'Error',
        text:  'Ocurrió un error desconocido al actualizar el widget.',
      });
      this.$notify({
        type: "danger",
        icon: "tim-icons icon-alert-circle-exc",
        message: "An unexpected error occurred"
      });
    }
  }
}


  }
};
</script>
