<template>
  <div ref="inicio" >
    
    <!-- WIDGET CONFIGURATOR -->
    <div class="row" >
      <card >



 
   <div>


    <!-- Formulario se muestra al hacer clic en el botón -->
    <div v-if="mostrarFormularioAddWidget" >
      <h3>Configuración del Widget</h3>

<div class="col-6"  v-if="mostrarFormularioAddWidget">

             
            <el-select 
              v-model="widgetType"
              class="select-success"
              placeholder="Selecciona un Widget"
              style="width: 100%;"
            >
              <el-option
                class="text-dark"
                value="numberchart"
                label="Number Chart INPUT <-"
              >
              </el-option>
              <el-option
                class="text-dark"
                value="indicator"
                label="Boolean Indicator INPUT <-"
              >
              </el-option>
              <el-option
                class="text-dark"
                value="map"
                label="Map INPUT <-"
              ></el-option>
              <el-option
                class="text-dark"
                value="switch"
                label="Switch OUTPUT ->"
              ></el-option>
              <el-option
                class="text-dark"
                value="button"
                label="Button OUTPUT ->"
              ></el-option>
              <el-option
                class="text-dark"
                value="sendNumber"
                label="Send Number OUTPUT ->"
              ></el-option>
            </el-select>
</div>
            <br />
            <br />
    </div>
  </div>



 <div class="row">


<div class="col-6" >


    <p>{{ selectedTemplate }}</p> 
   <br>
   <p>{{ selectedWidget }}</p>  

         <!-- WIDGET SELECTOR AND FORMS -->

         <!-- WIDGETS SELECTOR -->
            <br />
            <br />

<div v-if="(selectedWidget.widget  === 'numberchart' || widgetType === 'numberchart')  ">

              <base-input
                v-model="ncConfig.variableFullName"
                label="Var Name"
                type="text"
              >
              </base-input>

              <base-input v-model="ncConfig.unit" label="Unit" type="text">
              </base-input>

              <base-input
                v-model.number="ncConfig.decimalPlaces"
                label="Decimal Places"
                type="number"
              >
              </base-input>

              <base-input
                v-model="ncConfig.icon"
                label="Icon"
                type="text"
              ></base-input>

              <br />

              <base-input
                v-model.number="ncConfig.variableSendFreq"
                label="Send Freq"
                type="number"
              ></base-input>

              <br />

              <base-input
                v-model.number="ncConfig.chartTimeAgo"
                label="Chart Back Time (mins)"
                type="number"
              ></base-input>

              <br />

              <el-select
                v-model="ncConfig.class"
                class="select-success"
                placeholder="Select Class"
                style="width: 100%;"
              >
                <el-option
                  class="text-success"
                  value="success"
                  label="Success"
                ></el-option>
                <el-option
                  class="text-primary"
                  value="primary"
                  label="Primary"
                ></el-option>
                <el-option
                  class="text-warning"
                  value="warning"
                  label="Warning"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="danger"
                  label="Danger"
                ></el-option>
              </el-select>

              <br /><br /><br />

              <el-select
                v-model="ncConfig.column"
                class="select-success"
                placeholder="Seleccione el ancho de la columna"
                style="width: 100%;"
              >
                <el-option
                  class="text-dark"
                  value="col-3"
                  label="col-3"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-4"
                  label="col-4"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-5"
                  label="col-5"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-6"
                  label="col-6"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-7"
                  label="col-7"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-8"
                  label="col-8"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-9"
                  label="col-9"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-10"
                  label="col-10"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-11"
                  label="col-11"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-12"
                  label="col-12"
                ></el-option>
              </el-select>

              <br /><br />
            </div>

            <!-- FORM SWITCH TYPE -->
            <div v-if="selectedWidget.widget === 'switch' || widgetType === 'switch' ">
              <base-input
                v-model="iotSwitchConfig.variableFullName"
                label="Var Name"
                type="text"
              >
              </base-input>

              <base-input
                v-model="iotSwitchConfig.icon"
                label="Icon"
                type="text"
              ></base-input>

              <br />

              <el-select
                v-model="iotSwitchConfig.class"
                class="select-success"
                placeholder="Select Class"
                style="width: 100%;"
              >
                <el-option
                  class="text-success"
                  value="success"
                  label="Success"
                ></el-option>
                <el-option
                  class="text-primary"
                  value="primary"
                  label="Primary"
                ></el-option>
                <el-option
                  class="text-warning"
                  value="warning"
                  label="Warning"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="danger"
                  label="Danger"
                ></el-option>
              </el-select>

              <br /><br /><br />

              <el-select
                v-model="iotSwitchConfig.column"
                class="select-success"
                placeholder="Select Column Width"
                style="width: 100%;"
              >
                <el-option
                  class="text-dark"
                  value="col-3"
                  label="col-3"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-4"
                  label="col-4"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-5"
                  label="col-5"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-6"
                  label="col-6"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-7"
                  label="col-7"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-8"
                  label="col-8"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-9"
                  label="col-9"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-10"
                  label="col-10"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-11"
                  label="col-11"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-12"
                  label="col-12"
                ></el-option>
              </el-select>

              <br /><br />
            </div>

            <!-- FORM BUTTON TYPE -->
            <div v-if="selectedWidget.widget === 'button'  || widgetType === 'button' ">
              <base-input
                v-model="configButton.variableFullName"
                label="Var Name"
                type="text"
              >
              </base-input>

              <base-input
                v-model="configButton.message"
                label="Message to send"
                type="text"
              >
              </base-input>

              <base-input
                v-model="configButton.text"
                label="Button Text"
                type="text"
              >
              </base-input>

              <base-input
                v-model="configButton.icon"
                label="Icon"
                type="text"
              ></base-input>

              <br />

              <el-select
                v-model="configButton.class"
                class="select-success"
                placeholder="Select Class"
                style="width: 100%;"
              >
                <el-option
                  class="text-success"
                  value="success"
                  label="Success"
                ></el-option>
                <el-option
                  class="text-primary"
                  value="primary"
                  label="Primary"
                ></el-option>
                <el-option
                  class="text-warning"
                  value="warning"
                  label="Warning"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="danger"
                  label="Danger"
                ></el-option>
              </el-select>

              <br /><br /><br />

              <el-select
                v-model="configButton.column"
                class="select-success"
                placeholder="Select Column Width"
                style="width: 100%;"
              >
                <el-option
                  class="text-dark"
                  value="col-3"
                  label="col-3"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-4"
                  label="col-4"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-5"
                  label="col-5"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-6"
                  label="col-6"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-7"
                  label="col-7"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-8"
                  label="col-8"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-9"
                  label="col-9"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-10"
                  label="col-10"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-11"
                  label="col-11"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-12"
                  label="col-12"
                ></el-option>
              </el-select>

              <br /><br />
            </div>

            <!-- FORM SEND NUMBER TYPE -->
            <div v-if="selectedWidget.widget === 'sendNumber' || widgetType === 'sendNumber' ">
              <base-input
                v-model="sendNumberConfig.variableFullName"
                label="Nombre"
                type="text"
              >
              </base-input>

              <br />
              <base-input
                v-model="sendNumberConfig.message"
                label="Mensaje"
                type="text"
              >
              </base-input>

              <br />

              <p>Valor del Mensaje</p>
              <el-select
                v-model="sendNumberConfig.messageValue"
                label="Mensaje a Enviar"
                class="select-success"
                placeholder="Seleccione un Valor"
                style="width: 100%;"
              >
                <el-option
                  class="text-success"
                  value="1"
                  label="1"
                ></el-option>
                <el-option
                  class="text-primary"
                  value="2"
                  label="2"
                ></el-option>
                <el-option
                  class="text-warning"
                  value="3"
                  label="3"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="4"
                  label="4"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="5"
                  label="5"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="6"
                  label="6"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="7"
                  label="7"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="8"
                  label="8"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="9"
                  label="9"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="10"
                  label="10"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="11"
                  label="11"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="12"
                  label="12"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="13"
                  label="13"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="14"
                  label="14"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="15"
                  label="15"
                ></el-option>

              </el-select>

              <base-input
                v-model="sendNumberConfig.text"
                label="Texto del Boton"
                type="text"
              >
              </base-input>

              <base-input
                v-model="sendNumberConfig.icon"
                label="Icono"
                type="text"
              ></base-input>

              <br />

              <el-select
                v-model="sendNumberConfig.class"
                class="select-success"
                placeholder="Seleccione Clase"
                style="width: 100%;"
              >
                <el-option
                  class="text-success"
                  value="success"
                  label="Success"
                ></el-option>
                <el-option
                  class="text-primary"
                  value="primary"
                  label="Primary"
                ></el-option>
                <el-option
                  class="text-warning"
                  value="warning"
                  label="Warning"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="danger"
                  label="Danger"
                ></el-option>
              </el-select>

              <br /><br /><br />

              <el-select
                v-model="sendNumberConfig.column"
                class="select-success"
                placeholder="Seleccione ancoho de columna"
                style="width: 100%;"
              >
                <el-option
                  class="text-dark"
                  value="col-3"
                  label="col-3"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-4"
                  label="col-4"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-5"
                  label="col-5"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-6"
                  label="col-6"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-7"
                  label="col-7"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-8"
                  label="col-8"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-9"
                  label="col-9"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-10"
                  label="col-10"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-11"
                  label="col-11"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-12"
                  label="col-12"
                ></el-option>
              </el-select>

              <br /><br />
            </div>

            <!-- FORM INDICATOR TYPE -->
            <div v-if="selectedWidget.widget === 'indicator'  || widgetType === 'indicator'  ">

              <base-input
                v-model="iotIndicatorConfig.variableFullName"
                label="Nombre"
                type="text"
              >
              </base-input>

              <base-input
                v-model="iotIndicatorConfig.icon"
                label="Icono"
                type="text"
              ></base-input>

              <br />

              <base-input
                v-model="iotIndicatorConfig.variableSendFreq"
                label="Frecuencia de Envío"
                type="text"
              ></base-input>

              <br />

              <el-select
                v-model="iotIndicatorConfig.class"
                class="select-success"
                placeholder="Select Class"
                style="width: 100%;"
              >
                <el-option
                  class="text-success"
                  value="success"
                  label="Success"
                ></el-option>
                <el-option
                  class="text-primary"
                  value="primary"
                  label="Primary"
                ></el-option>
                <el-option
                  class="text-warning"
                  value="warning"
                  label="Warning"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="danger"
                  label="Danger"
                ></el-option>
              </el-select>

              <br /><br /><br />

              <el-select
                v-model="iotIndicatorConfig.column"
                class="select-success"
                placeholder="Select Column Width"
                style="width: 100%;"
              >
                <el-option
                  class="text-dark"
                  value="col-2"
                  label="col-2"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-3"
                  label="col-3"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-4"
                  label="col-4"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-5"
                  label="col-5"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-6"
                  label="col-6"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-7"
                  label="col-7"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-8"
                  label="col-8"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-9"
                  label="col-9"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-10"
                  label="col-10"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-11"
                  label="col-11"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-12"
                  label="col-12"
                ></el-option>
              </el-select>

              <br /><br />
            </div>

            <!-- FORM MAP TYPE -->
            <div v-if="selectedWidget.widget  === 'map'  || widgetType === 'map' ">

              <base-input
                v-model="iotMapConfig.variableFullName"
                label="Nombre"
                type="text"
              >
              </base-input>

              <base-input
                v-model="iotMapConfig.icon"
                label="Icono"
                type="text"
              ></base-input>

              <br />

              <base-input
                v-model="iotMapConfig.variableSendFreq"
                label="Frecuencia de Envío"
                type="text"
              ></base-input>

              <br />
              

              <el-select
                v-model="iotMapConfig.class"
                class="select-success"
                placeholder="Select Class"
                style="width: 100%;"
              >
                <el-option
                  class="text-success"
                  value="success"
                  label="Success"
                ></el-option>
                <el-option
                  class="text-primary"
                  value="primary"
                  label="Primary"
                ></el-option>
                <el-option
                  class="text-warning"
                  value="warning"
                  label="Warning"
                ></el-option>
                <el-option
                  class="text-danger"
                  value="danger"
                  label="Danger"
                ></el-option>
              </el-select>

              <br /><br /><br />

              <el-select
                v-model="iotMapConfig.column"
                class="select-success"
                placeholder="Select Column Width"
                style="width: 100%;"
              >
                <el-option
                  class="text-dark"
                  value="col-2"
                  label="col-2"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-3"
                  label="col-3"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-4"
                  label="col-4"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-5"
                  label="col-5"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-6"
                  label="col-6"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-7"
                  label="col-7"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-8"
                  label="col-8"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-9"
                  label="col-9"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-10"
                  label="col-10"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-11"
                  label="col-11"
                ></el-option>
                <el-option
                  class="text-dark"
                  value="col-12"
                  label="col-12"
                ></el-option>
              </el-select>

              <br /><br />
            </div>

</div>



          <!-- WIDGET PREVIEW -->
          <div class="col-6">
            <Rtnumberchart
              v-if="selectedWidget.widget === 'numberchart' || widgetType === 'numberchart'  "
              :config="ncConfig"
            ></Rtnumberchart>
            <Iotswitch
              v-if="selectedWidget.widget  === 'switch' || widgetType === 'switch' "
              :config="iotSwitchConfig"
            ></Iotswitch>
            <Iotbutton
              v-if="selectedWidget.widget  === 'button' || widgetType === 'button'"
              :config="configButton"
            ></Iotbutton>
            <Iotindicator
              v-if="selectedWidget.widget  === 'indicator' || widgetType === 'indicator '"
              :config="iotIndicatorConfig"
            ></Iotindicator>
            <IotMap
              v-if="selectedWidget.widget  === 'map' || widgetType === 'map'"
              :config="iotMapConfig"
            ></IotMap>
            <IotSendNumber
              v-if="selectedWidget.widget  === 'sendNumber' || widgetType === 'senNumber'"
              :config="sendNumberConfig"
            ></IotSendNumber>
          
          </div>
        </div>

        <!-- UPDATE WIDGET BUTTON -->
        <div class="row " v-if="!mostrarFormularioAddWidget">

          <div class="col-6">

            <base-button
              native-type="submit"
              type="success"
              class="mb-3 animation-on-hover "
              size="lg"
              @click="mostrarFormularioAddWidget = true, selectedWidget = '' "
              :disabled="!selectedTemplate.name || selectedTemplate.name.trim() === ''"

            >
              Añadir Nuevo Widget
            </base-button>
          </div>

          <div class="col-6 pull-right">

            
            <base-button
              native-type="submit"
              type="primary"
              class="mb-3 animation-on-hover"
              size="lg"
              @click="updateWidget(selectedWidget)"
              :disabled="!selectedWidget.variable || selectedWidget.variable.trim() === ''"

            >
              Actualizar Widget
            </base-button>

             <base-button 
              class="mb-3 pull-right animation-on-hover" 
             type="default"
             size="lg"
            @click="editCancel()"
             >
             Cancelar Actualizacion
            </base-button>

          </div>
        </div>

        <!-- EDIT ADD WIDGET BUTTON -->
        <div class="row  pull-right" v-if="mostrarFormularioAddWidget">


            
            <base-button
              native-type="submit"
              type="success"
              class="mb-3 animation-on-hover"
              size="lg"
              @click="addNewWidget()"
              :disabled="!widgetType || widgetType.trim() === ''"

            >
              Agregar Widget
            </base-button>

             <base-button 
              class="mb-3 animation-on-hover" 
             type="default"
             size="lg"
            @click="mostrarFormularioAddWidget = false , widgetType= '' "
             >
             Cancelar
            </base-button>

        </div>
      </card>
    </div> 

  <div>


    <!-- Vista previa del template con los widgets -->
  </div>

    <h2 ref="selectedTemplate" class="text-center" >Preview de la plantilla seleccionada para edición</h2>

  <div  class="row" v-if="selectedTemplate">
  
<div class="container">
  <div class="row col-12">
    <draggable v-model="selectedTemplate.widgets" @end="" >
      <div v-for="(widget, index) in selectedTemplate.widgets" :key="widget.variable" class="widget-card">
        <span class="btn btn-lg animation-on-hover drag-handle">☰ {{ widget.variableFullName }}</span> <!-- Icono para arrastrar -->
      </div>
    </draggable>
  </div>
</div>

<div class="row col-12">
<div v-for="(widget, Index) in selectedTemplate.widgets" :key="widget.variable" :class="[widget.column]">

    <i class="fa fa-trash text-warning pull-right" @click="deleteWidget(Index)" style="margin-bottom: 10px;"></i>
    <i class="fa fa-edit text-primary pull-right" @click="editWidget(selectedTemplate, widget)" style="margin-bottom: 10px; margin-right: 10px;"></i>

    <Rtnumberchart v-if="widget.widget == 'numberchart'" :config="widget"></Rtnumberchart>
    <Iotswitch v-if="widget.widget == 'switch'" :config="widget"></Iotswitch>
    <Iotbutton v-if="widget.widget == 'button'" :config="widget"></Iotbutton>
    <Iotindicator v-if="widget.widget == 'indicator'" :config="widget"></Iotindicator>
    <IotMap v-if="widget.widget == 'map'" :config="widget"></IotMap>
    <IotSendNumber v-if="widget.widget == 'sendNumber'" :config="widget"></IotSendNumber>
  </div>

</div>
</div>
<p v-else>Selecciona una plantilla para ver los widgets.</p>

    <!-- SAVE TEMPLATE FORM-->
    <div class="row" >
      <card>
        <div slot="header">
          <h4 class="card-title">Actualizar Plantilla</h4>
        </div>

        <div class="row">
          <base-input
            class="col-4"
            v-model="templateName"
            label="Template Name"
            type="text"
          >
          </base-input>

          <base-input
            class="col-8"
            v-model="templateDescription"
            label="Template Description"
            type="text"
          >
          </base-input>
        </div>

        <br /><br />

        <div class="row">
          <div class="col-12" >

            <base-button v-if = (templateEdition)
              class="mb-3 pull-right animation-on-hover" 
             type="default"
             size="lg"
            @click="editCancel()"
             >
             Cancelar
            </base-button>

            <base-button v-if = (templateEdition) 
              native-type="submit"
              type="primary"
              class="mb-3 pull-right animation-on-hover" 
              size="lg"
              @click="updateTemplate(selectedTemplate, templateName, templateDescription)"
              :disabled="!selectedTemplate.name || selectedTemplate.name.trim() === ''"
            >
              Actualizar Plantilla 
            </base-button>

            <base-button v-if = (!templateEdition)
              native-type="submit"
              type="primary"
              class="mb-3 pull-right"
              size="lg"
              @click="saveTemplate()"
              :disabled="widgets.length == 0"
            >
              Guardar Plantilla
            </base-button>



          </div>
        </div>
      </card>
    </div>

    <!-- TEMPLATES TABLE -->
    <div class="row">
      <card>
        <div slot="header">
          <h4 class="card-title">Templates</h4>
        </div>

        <div class="row">
          <el-table :data="templates">
            <el-table-column min-width="50" label="#" align="center">
              <div class="photo" slot-scope="{ row, $index }">
                {{ $index + 1 }}
              </div>
            </el-table-column> 

            <el-table-column prop="name" label="Name"></el-table-column>

            <el-table-column
              prop="description"
              label="Description"
            ></el-table-column>

            <el-table-column
              prop="widgets.length"
              label="Widgets"
            ></el-table-column>

            <el-table-column header-align="right" align="right" label="Actions">
              <div
                slot-scope="{ row, $index }"
                class="text-right table-actions"
              >

                <el-tooltip content="Edit" effect="light" :open-delay="300" placement="top">
                  <base-button @click="editTemplate(row, $index)" type="success" icon size="sm">
                    <i class="fa fa-edit"></i>
                  </base-button>
                </el-tooltip>


                <el-tooltip
                  content="Delete"
                  effect="light"
                  :open-delay="300"
                  placement="top"
                >
                  <base-button
                    @click="deleteTemplate(row)"
                    type="danger"
                    icon
                    size="sm"
                    class="btn-link"
                  >
                    <i class="tim-icons icon-simple-remove "></i>
                  </base-button>
                  
                </el-tooltip>

              </div>
            </el-table-column>
          </el-table>
        </div>
      </card>
    </div>
  </div>
</template>

<script>
import { Table, TableColumn } from "element-ui";
import { Select, Option } from "element-ui";
import Swal from 'sweetalert2';
import 'sweetalert2/dist/sweetalert2.css'
import draggable from 'vuedraggable';

export default {
  middleware: "authenticated",
  components: {
    draggable,
    [Table.name]: Table,
    [TableColumn.name]: TableColumn,
    [Option.name]: Option,
    [Select.name]: Select
  },

  data() {
    return {
      widgets: [],
      templates: [],
      widgetType: "",
      templateName: "",
      templateDescription: "",

    selectedTemplate: {},  // Para almacenar el template seleccionado
    selectedWidget: {}, 
    mostrarFormularioAddWidget: false, // Controla la visibilidad del cuadro de configuración
    templateEdition: false,
      templateName: "",
    templateDescription: "",  
    

      ncConfig: {
        userId: "sampleuserid",
        selectedDevice: {
          name: "Home",
          dId: "8888"
        },
        variableFullName: "temperature",
        variable: "varname",
        variableType: "input",
        variableSendFreq: "30",
        unit: "Watts",
        class: "success",
        column: "col-12",
        decimalPlaces: 2,
        widget: "numberchart",
        icon: "fa-sun",
        chartTimeAgo: 60,
        demo: true
      },

      iotSwitchConfig: {
        userId: "userid",
        selectedDevice: {
          name: "Home",
          dId: "8888"
        },
        variableFullName: "Luz",
        variable: "varname",
        variableType: "output",
        class: "danger",
        widget: "switch",
        icon: "fa-bath",
        column: "col-6"
      },


      iotIndicatorConfig: {
        userId: "userid",
        selectedDevice: {
          name: "Home",
          dId: "8888"
        },
        variableFullName: "temperature",
        variable: "varname",
        variableType: "input",
        variableSendFreq: "30",
        class: "success",
        widget: "indicator",
        icon: "fa-bath",
        column: "col-6"
      },

      configButton: {
        userId: "userid",
        selectedDevice: {
          name: "Home",
          dId: "8888",
          templateName: "Power Sensor",
          templateId: "984237562348756ldksjfh",
          saverRule: false
        },
        variableFullName: "Pump",
        variable: "var1",
        variableType: "output",
        icon: "fa-sun",
        column: "col-4",
        widget: "button",
        class: "danger",
        message: "{'pumpstatus': 'stop'}"
      },

      sendNumberConfig: {
        userId: "userid",
        selectedDevice: {
          name: "Home",
          dId: "8888",
          templateName: "Power Sensor",
          templateId: "984237562348756ldksjfh",
          saverRule: false
        },
        variableFullName: "Pump",
        variable: "var1",
        variableType: "output",
        icon: "fa-caret-square-right",
        column: "col-4",
        widget: "sendNumber",
        class: "danger",
        message: "Tiempo en minutos", 
        messageValue: "", 
        text: "Enviar"

      },

      iotMapConfig: {
        userId: "userid",
        selectedDevice: {
          name: "Home",
          dId: "8888"
        },
        variableFullName: "Mapa",
        variable: "varname",
        variableType: "input",
        variableSendFreq: "30",
        class: "success",
        widget: "map",
        icon: "fas fa-map",
        column: "col-6"
      },


    };
  },

  watch: {

    selectedWidget: {
      handler(newVal) {
        if (newVal) {

          this.templateName = this.selectedTemplate.name
          this.templateDescription = this.selectedTemplate.description
        if (newVal.widget == "numberchart")
         {
        this.ncConfig.userId = newVal.userId,
        this.ncConfig.selectedDevice = {
          name : newVal.selectedDevice.name,
        dId : newVal.selectedDevice.dId
        },
        this.ncConfig.variableFullName= newVal.variableFullName,
        this.ncConfig.variable= newVal.variable,
        this.ncConfig.variableType= newVal.variableType,
        this.ncConfig.variableSendFreq= newVal.variableSendFreq,
        this.ncConfig.unit= newVal.unit,
        this.ncConfig.class= newVal.class,
        this.ncConfig.column= newVal.column,
        this.ncConfig.decimalPlaces= newVal.decimalPlaces,
        this.ncConfig.widget= newVal.widget,
        this.ncConfig.icon= newVal.icon,
        this.ncConfig.chartTimeAgo= newVal.chartTimeAgo,
        this.ncConfig.demo= newVal.demo
      }

      if (newVal.widget == "switch") {
        this.iotSwitchConfig.userId= newVal.userId,
        this.iotSwitchConfig.selectedDevice= {
          name:newVal.selectedDevice.name,
          dId: newVal.selectedDevice.dId
        },
        this.iotSwitchConfig.variableFullName= newVal.variableFullName,
        this.iotSwitchConfig.variable= newVal.variable,
        this.iotSwitchConfig.variableType= newVal.variableType,
        this.iotSwitchConfig.class= newVal.class,
        this.iotSwitchConfig.widget= newVal.widget,
        this.iotSwitchConfig.icon= newVal.icon,
        this.iotSwitchConfig.column= newVal.column
      }


      if (newVal.widget == "indicator") {
        this.iotIndicatorConfig.userId = newVal.userId,
        this.iotIndicatorConfig.selectedDevice= {
          name: newVal.selectedDevice.name,
          dId: newVal.selectedDevice.dId
        },
        this.iotIndicatorConfig.variableFullName= newVal.variableFullName,
        this.iotIndicatorConfig.variable= newVal.variable,
        this.iotIndicatorConfig.variableType= newVal.variableType,
        this.iotIndicatorConfig.variableSendFreq= newVal.variableSendFreq,
        this.iotIndicatorConfig.class= newVal.class,
        this.iotIndicatorConfig.widget= newVal.widget,
        this.iotIndicatorConfig.icon= newVal.icon,
        this.iotIndicatorConfig.column= newVal.column
      }

      if (newVal.widget == "button") {
        this.configButton.userId = newVal.userId,
        this.configButton.selectedDevice= {
          name: newVal.selectedDevice.name,
          dId: newVal.selectedDevice.dId,
          templateName: newVal.selectedDevice.templateName,
          templateId: newVal.selectedDevice.templateId,
          saverRule: newVal.selectedDevice.saverRule
        },
        this.configButton.variableFullName= newVal.variableFullName,
        this.configButton.variable= newVal.variable,
        this.configButton.variableType= newVal.variableType,
        this.configButton.icon= newVal.icon,
        this.configButton.column= newVal.column,
        this.configButton.widget= newVal.widget,
        this.configButton.class= newVal.class,
        this.configButton.message= newVal.message
      }

      if (newVal.widget == "sendNumber") {
        this.sendNumberConfig.userId = newVal.userId,
        this.sendNumberConfig.selectedDevice =  {
          name: newVal.selectedDevice.name,
          dId: newVal.selectedDevice.dId,
          templateName: newVal.selectedDevice.templateName,
          templateId: newVal.selectedDevice.templateId,
          saverRule: newVal.selectedDevice.saverRule
        },
        this.sendNumberConfig.variableFullName= newVal.variableFullName,
        this.sendNumberConfig.variable= newVal.variable,
        this.sendNumberConfig.variableType= newVal.variableType,
        this.sendNumberConfig.icon= newVal.icon,
        this.sendNumberConfig.column= newVal.comumn,
        this.sendNumberConfig.widget= newVal.widget
        this.sendNumberConfig.class=newVal.class,
        this.sendNumberConfig.message= newVal.message, 
        this.sendNumberConfig.messageValue= newVal.messageValue, 
        this.sendNumberConfig.text= newVal.text

      }

      if (newVal.widget == "map")  {
        this.iotMapConfig.userId= newVal.userId,
        this.iotMapConfig.selectedDevice= {
          name: newVal.selectedDevice.name,
          dId: newVal.selectedDevice.dId
        },
        this.iotMapConfig.variableFullName= newVal.variableFullName,
        this.iotMapConfig.variable= newVal.variable,
        this.iotMapConfig.variableType= newVal.variableType,
        this.iotMapConfig.variableSendFreq= newVal.variableSendFreq,
        this.iotMapConfig.class= newVal.class,
        this.iotMapConfig.widget= newVal.widget,
        this.iotMapConfig.icon= newVal.icon,
        this.iotMapConfig.column= newVal.column
      }



      console.log("Plantilla actualizada:", this.selectedTemplate.widgets);

          
        }
      },
      deep: true,
      immediate: true, // Para que se ejecute al montar el componente

    },
    
  },

  mounted() {
    this.getTemplates();
    this.getToken();
  },

  methods: {

    getToken() {



  },
    
    // Editar widget - Carga datos en selectedTemplate y selectedWidget, activa edición
    editWidget(selectedTemplate, widget) {
      this.widgetType = "";
      this.mostrarFormularioAddWidget = false;
      this.selectedTemplate = { ...selectedTemplate }; // Clonamos el objeto
      this.selectedWidget = widget;
      this.templateEdition = true;


  // Hacer scroll al inicio de la página
  //window.scrollTo({
  //  top: 0,
  //  behavior: "smooth"
  //});


        // Hacer scroll al div donde se edita el widget
  this.$refs.home.scrollIntoView({ behavior: "smooth" });

    },

    // Editar widget - Carga datos en selectedTemplate y selectedWidget, activa edición
    editTemplate(widget) {
      this.selectedTemplate = { ...widget }; // Clonamos el objeto
      this.selectedWidget = {};
      this.templateEdition = true;
      this.widgetType = "";
      this.mostrarFormularioAddWidget = false;
      this.$refs.selectedTemplate.scrollIntoView({ behavior: "smooth" });
      console.log(this.selectedTemplate);

    },

    // Editar widget - Carga datos en selectedTemplate y selectedWidget, activa edición
    editCancel() {
      this.selectedTemplate = {}; // Clonamos el objeto
      this.selectedWidget = {};
      this.templateEdition = false;
      this.mostrarFormularioAddWidget = false;

   //Hacer scroll al inicio de la página
  window.scrollTo({
    top: 0,
    behavior: "smooth"
  });
    },

  saveTemplateChanges() {
    if (this.currentTemplateIndex !== null) {
      this.templates[this.currentTemplateIndex] = { ...this.editForm };
    }
    this.editModalVisible = false; // 🔥 Aquí cerramos el modal
  },

    loadWidgets() {
      this.selectedWidget = "";
      this.widgetType = "";
      this.templateName = this.selectedTemplate.name;
      this.templateDescription = this.selectedTemplate.description;


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
        this.$notify({
          type: "danger",
          icon: "tim-icons icon-alert-circle-exc",
          message: "Error getting templates..."
        });
        console.log(error);
        return;
      }
    },

    //Save Template
    async saveTemplate() {
      const axiosHeaders = {
        headers: {
          token: this.$store.state.auth.token
        }
      };


      const toSend = {
        template: {
          name: this.templateName,
          description: this.templateDescription,
          widgets: this.widgets
        }
      };

      try {
        const res = await this.$axios.post("/template", toSend, axiosHeaders);

        if (res.data.status == "success") {
          this.$notify({
            type: "success",
            icon: "tim-icons icon-alert-circle-exc",
            message: "Template created!"
          });
          this.getTemplates();

          this.widgets = [];
        }
      } catch (error) {
        this.$notify({
          type: "danger",
          icon: "tim-icons icon-alert-circle-exc",
          message: "Error creating template..."
        });
        console.log(error);
        return;
      }
    },

    //Delete Template
    async deleteTemplate(template) {

      
      const axiosHeaders = {
        headers: {
          token: this.$store.state.auth.token
        },
        params:{
          templateId:template._id
        }
      };


      try {

        const res = await this.$axios.delete("/template", axiosHeaders);

        console.log(res.data)

        if (res.data.status == "fail" && res.data.error == "template in use") {

          this.$notify({
            type: "danger",
            icon: "tim-icons icon-alert-circle-exc",
            message: template.name + " is in use. First remove the devices linked to the template!"
          });
          
          return;
        }

        if (res.data.status == "success") {
          this.$notify({
            type: "success",
            icon: "tim-icons icon-check-2",
            message: template.name + " was deleted!"
          });
          
          this.getTemplates();
        }
      } catch (error) {
        this.$notify({
          type: "danger",
          icon: "tim-icons icon-alert-circle-exc",
          message: "Error getting templates..."
        });
        console.log(error);
        return;
      }
    },

    //Add Widget
    addNewWidget() {
      if (this.widgetType == "numberchart") {
        this.ncConfig.variable = this.makeid(10);
        if (this.selectedTemplate) {
        this.selectedTemplate.widgets.push(JSON.parse(JSON.stringify(this.ncConfig)));
      }
        this.widgets.push(JSON.parse(JSON.stringify(this.ncConfig)));
      }

      if (this.widgetType == "switch") {
        this.iotSwitchConfig.variable = this.makeid(10);
        if (this.selectedTemplate) {
        this.selectedTemplate.widgets.push(JSON.parse(JSON.stringify(this.iotSwitchConfig)));
      }
        this.widgets.push(JSON.parse(JSON.stringify(this.iotSwitchConfig)));
      }

      if (this.widgetType == "button") {
        this.configButton.variable = this.makeid(10);
        this.selectedTemplate.widgets.push(JSON.parse(JSON.stringify(this.configButton)));
        this.widgets.push(JSON.parse(JSON.stringify(this.configButton)));
      }

      if (this.widgetType == "indicator") {
        this.iotIndicatorConfig.variable = this.makeid(10);
        this.selectedTemplate.widgets.push(JSON.parse(JSON.stringify(this.iotIndicatorConfig)));
        this.widgets.push(JSON.parse(JSON.stringify(this.iotIndicatorConfig)));
      }

      if (this.widgetType == "map") {
        this.iotMapConfig.variable = this.makeid(10);
        this.selectedTemplate.widgets.push(JSON.parse(JSON.stringify(this.iotMapConfig)));
        this.widgets.push(JSON.parse(JSON.stringify(this.iotMapConfig)));
      }

      if (this.widgetType == "sendNumber") {
        this.sendNumberConfig.variable = this.makeid(10);
        this.selectedTemplate.widgets.push(JSON.parse(JSON.stringify(this.sendNumberConfig)));
        this.widgets.push(JSON.parse(JSON.stringify(this.sendNumberConfig)));
      }

      this.widgetType = "";

    },

  async updateWidget(selectedWidget) {
    try {
      // Validación de entrada
      if (!selectedWidget || !this.selectedTemplate?.widgets) {
        throw new Error("selectedWidget o selectedTemplate.widgets no están definidos");
      }

      // Encontrar el índice del widget
      const index = this.selectedTemplate.widgets.findIndex(w => w.variable === selectedWidget.variable);
      if (index === -1) {
        throw new Error("Widget no encontrado en la lista");
      }

      // Determinar el tipo de widget
      let updateWidget;
      switch (selectedWidget.widget) {
        case "numberchart":
          updateWidget = this.ncConfig;
          break;
        case "switch":
          updateWidget = this.iotSwitchConfig;
          break;
        case "button":
          updateWidget = this.configButton;
          break;
        case "indicator":
          updateWidget = this.iotIndicatorConfig;
          break;
        case "map":
          updateWidget = this.iotMapConfig;
          break;
        case "sendNumber":
          updateWidget = this.sendNumberConfig;
          break;
        default:
          throw new Error(`Tipo de widget no reconocido: ${selectedWidget.widget}`);
      }

      // Validar que updateWidget tenga un valor asignado
      if (!updateWidget) {
        throw new Error("No se pudo determinar la configuración del widget");
      }

      // Reemplazar el widget en su posición
      this.selectedTemplate.widgets.splice(index, 1, JSON.parse(JSON.stringify(updateWidget)));

      // Mostrar alerta de éxito
      await Swal.fire({
        icon: 'success',
        title: 'Widget actualizado correctamente',
        text: 'El widget se ha actualizado correctamente. Debe actualizar la plantilla en el botón de abajo para guardar la información',
      });
    } catch (error) {
      // Mostrar alerta de error
      await Swal.fire({
        icon: 'error',
        title: 'Error',
        text: error.message || 'Ocurrió un error al actualizar el widget.',
      });
    }
  },


    //Delete Widget
    async deleteWidget(index) {


    // Mostrar mensaje de confirmación con SweetAlert2
    const result = await Swal.fire({
      title: '¿Estás seguro?',
      text: "¿Deseas eliminar este widget?",
      icon: 'warning',
      showCancelButton: true,
      confirmButtonColor: '#3085d6',
      cancelButtonColor: '#d33',
      confirmButtonText: 'Sí, eliminar',
      cancelButtonText: 'Cancelar'
    });

    // Si el usuario confirma la eliminación
    if (result.isConfirmed) {
      // Lógica para eliminar el widget
      if (index !== -1) {

      if (this.selectedTemplate){
      this.selectedTemplate.widgets.splice(index, 1);
      }
      this.widgets.splice(index, 1);

        Swal.fire(
          'Eliminado!',
          'El widget ha sido eliminado. Guarda la plantilla para confirmar la Actualizacion',
          'success'
        );
      } else {
        throw new Error("Widget no encontrado en la lista");
      }
    } else {
      // Si el usuario cancela
      Swal.fire(
        'Cancelado',
        'La eliminación ha sido cancelada.',
        'error'
      );

    }
  },

      //Update Template
    async updateTemplate(template, templateName, templateDescription) {
      const axiosHeaders = {
          headers: {
              token: this.$store.state.auth.token
          }
      };
    
      const updatedData = {
          templateId: template._id,
          updatedData: {
              name: templateName,
              description: templateDescription,
              widgets: template.widgets
          }
      };
    
      try {
          const res = await this.$axios.put("/template", updatedData, axiosHeaders);
    
          if (res.data && res.data.status === "success") {
              this.$notify({
                  type: "success",
                  icon: "tim-icons icon-check-2",
                  message: template.name + " was updated!"
              });
            
              this.getTemplates();
          } else {
              throw new Error("Failed to update template");
          }
      } catch (error) {
          console.error("Error updating template:", error);
      
          this.$notify({
              type: "danger",
              icon: "tim-icons icon-alert-circle-exc",
              message: "Error updating template..."
          });
      }
  },

      saveOrder() {
      console.log("Nuevo orden guardado:", this.selectedTemplate);
    }, 

    makeid(length) {
      var result = "";
      var characters =
        "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
      var charactersLength = characters.length;
      for (var i = 0; i < length; i++) {
        result += characters.charAt(
          Math.floor(Math.random() * charactersLength)
        );
      }
      return result;
    }
  }
};


</script>
