<template>
  <div ref="inicio">
    <!-- WIDGET CONFIGURATOR -->

    <div class="row">
      <card>

        <div class="row" v-if="templateEdition && !mostrarFormularioAddWidget">

          <div slot="header" class="row col-12 text-center">
            <h1 class="card-title text-center">EDICIÓN DE PLANTILLAS</h1>
          </div>

          <!-- Dropdown para seleccionar la plantilla -->

          <div class="col-6">
            <label for="selectedTemplate">Selecciona una plantilla:</label>
            <el-select v-model="selectedTemplate" class="select-danger" placeholder="Selecciona una plantilla"
              @change="loadWidgets" style="width: 100%;" filterable clearable value-key="_id">
              <el-option v-for="template in templates" :key="template._id" :label="template.name" :value="template"
                class="text-dark" style="width: 100%;">
              </el-option>
            </el-select>
            <!-- Dropdown para seleccionar el widget -->

            <label for="template-select">Selecciona widget:</label>
            <el-select v-model="selectedWidget" class="select-warning" placeholder="Selecciona un widget" @change=""
              style="width: 100%;" filterable clearable value-key="variable">
              <el-option v-for="widget in selectedTemplate.widgets" :key="widget.variable"
                :label="widget.widget + ' ' + widget.variableFullName + ' ' + widget.variable" :value="widget"
                class="text-dark" style="width: 100%;">
              </el-option>
            </el-select>
            <br><br><br>
          </div>



        </div>

        <div slot="header" class="row col-12 text-center">
          <h2 v-if=(!templateEdition) class="card-title text-center">Crear Widgets </h2>
          <h2 v-if=(mostrarFormularioAddWidget) class="card-title text-center">Edicion de Plantillas Crear Widgets </h2>

        </div>

        <div class="row">
          <!-- WIDGET SELECTOR AND FORMS -->
          <div class="col-6">
            <!-- WIDGETS SELECTOR -->

            <el-select v-if="mostrarFormularioAddWidget || !templateEdition" v-model="widgetType" class="select-success"
              placeholder="Selecciona un Widget" style="width: 100%;">
              <el-option class="text-dark" value="numberchart" label="Number Chart INPUT <-">
              </el-option>
              <el-option class="text-dark" value="indicator" label="Boolean Indicator INPUT <-">
              </el-option>
              <el-option class="text-dark" value="map" label="Map INPUT <-"></el-option>
              <el-option class="text-dark" value="switch" label="Switch OUTPUT ->"></el-option>
              <el-option class="text-dark" value="button" label="Button OUTPUT ->"></el-option>
              <el-option class="text-dark" value="sendNumber" label="Send Number OUTPUT ->"></el-option>
            </el-select>
            <br />
            <br />


            <!-- WIDGET SELECTOR AND FORMS -->

            <!-- WIDGETS SELECTOR -->
            <br />
            <br />

            <div v-if="(selectedWidget.widget === 'numberchart' || widgetType === 'numberchart')">

              <base-input v-model="ncConfig.variableFullName" label="Var Name" type="text">
              </base-input>

              <base-input v-model="ncConfig.unit" label="Unit" type="text">
              </base-input>

              <base-input v-model.number="ncConfig.decimalPlaces" label="Decimal Places" type="number">
              </base-input>

              <p>Selecciona el Icono</p>
              <el-select class="select-success" v-model="ncConfig.icon" placeholder="Selecciona o ingresa un Icono"
                filterable @change="" style="width: 100%;">

                <el-option v-for="option in iconsList" :value="option.value" :label="option.label" :key="option.label">
                  <!-- Slot personalizado para mostrar el icono y el label -->
                  <span style="display: flex; align-items: center;">
                    <i :class="option.value" style="margin-right: 10px;"></i>
                    <span>{{ option.label }}</span>
                  </span>
                </el-option>

              </el-select>

              <br />

              <base-input v-model.number="ncConfig.variableSendFreq" label="Send Freq" type="number"></base-input>

              <br />

              <base-input v-model.number="ncConfig.chartTimeAgo" label="Chart Back Time (mins)"
                type="number"></base-input>

              <br />

              <el-select v-model="ncConfig.class" class="select-success" placeholder="Select Class"
                style="width: 100%;">
                <el-option class="text-success" value="success" label="Success"></el-option>
                <el-option class="text-primary" value="primary" label="Primary"></el-option>
                <el-option class="text-warning" value="warning" label="Warning"></el-option>
                <el-option class="text-danger" value="danger" label="Danger"></el-option>
              </el-select>

              <br /><br /><br />

              <el-select v-model="ncConfig.column" class="select-success"
                placeholder="Seleccione el ancho de la columna" style="width: 100%;">
                <el-option class="text-dark" value="col-3" label="col-3"></el-option>
                <el-option class="text-dark" value="col-4" label="col-4"></el-option>
                <el-option class="text-dark" value="col-5" label="col-5"></el-option>
                <el-option class="text-dark" value="col-6" label="col-6"></el-option>
                <el-option class="text-dark" value="col-7" label="col-7"></el-option>
                <el-option class="text-dark" value="col-8" label="col-8"></el-option>
                <el-option class="text-dark" value="col-9" label="col-9"></el-option>
                <el-option class="text-dark" value="col-10" label="col-10"></el-option>
                <el-option class="text-dark" value="col-11" label="col-11"></el-option>
                <el-option class="text-dark" value="col-12" label="col-12"></el-option>
              </el-select>

              <br /><br />
            </div>

            <!-- FORM SWITCH TYPE -->
            <div v-if="selectedWidget.widget === 'switch' || widgetType === 'switch'">
              <base-input v-model="iotSwitchConfig.variableFullName" label="Var Name" type="text">
              </base-input>

              <p>Selecciona el Icono</p>
              <el-select class="select-success" v-model="iotSwitchConfig.icon"
                placeholder="Selecciona o ingresa un Icono" filterable @change="" style="width: 100%;">

                <el-option v-for="option in iconsList" :value="option.value" :label="option.label" :key="option.label">
                  <!-- Slot personalizado para mostrar el icono y el label -->
                  <span style="display: flex; align-items: center;">
                    <i :class="option.value" style="margin-right: 10px;"></i>
                    <span>{{ option.label }}</span>
                  </span>
                </el-option>

              </el-select>

              <br />

              <el-select v-model="iotSwitchConfig.class" class="select-success" placeholder="Select Class"
                style="width: 100%;">
                <el-option class="text-success" value="success" label="Success"></el-option>
                <el-option class="text-primary" value="primary" label="Primary"></el-option>
                <el-option class="text-warning" value="warning" label="Warning"></el-option>
                <el-option class="text-danger" value="danger" label="Danger"></el-option>
              </el-select>

              <br /><br /><br />

              <el-select v-model="iotSwitchConfig.column" class="select-success" placeholder="Select Column Width"
                style="width: 100%;">
                <el-option class="text-dark" value="col-3" label="col-3"></el-option>
                <el-option class="text-dark" value="col-4" label="col-4"></el-option>
                <el-option class="text-dark" value="col-5" label="col-5"></el-option>
                <el-option class="text-dark" value="col-6" label="col-6"></el-option>
                <el-option class="text-dark" value="col-7" label="col-7"></el-option>
                <el-option class="text-dark" value="col-8" label="col-8"></el-option>
                <el-option class="text-dark" value="col-9" label="col-9"></el-option>
                <el-option class="text-dark" value="col-10" label="col-10"></el-option>
                <el-option class="text-dark" value="col-11" label="col-11"></el-option>
                <el-option class="text-dark" value="col-12" label="col-12"></el-option>
              </el-select>

              <br /><br />
            </div>

            <!-- FORM BUTTON TYPE -->
            <div v-if="selectedWidget.widget === 'button' || widgetType === 'button'">
              <base-input v-model="configButton.variableFullName" label="Var Name" type="text">
              </base-input>

              <base-input v-model="configButton.message" label="Message to send" type="text">
              </base-input>

              <base-input v-model="configButton.text" label="Button Text" type="text">
              </base-input>

              <p>Selecciona el Icono</p>
              <el-select class="select-success" v-model="configButton.icon" placeholder="Selecciona o ingresa un Icono"
                filterable @change="" style="width: 100%;">

                <el-option v-for="option in iconsList" :value="option.value" :label="option.label" :key="option.label">
                  <!-- Slot personalizado para mostrar el icono y el label -->
                  <span style="display: flex; align-items: center;">
                    <i :class="option.value" style="margin-right: 10px;"></i>
                    <span>{{ option.label }}</span>
                  </span>
                </el-option>

              </el-select>

              <br />

              <el-select v-model="configButton.class" class="select-success" placeholder="Select Class"
                style="width: 100%;">
                <el-option class="text-success" value="success" label="Success"></el-option>
                <el-option class="text-primary" value="primary" label="Primary"></el-option>
                <el-option class="text-warning" value="warning" label="Warning"></el-option>
                <el-option class="text-danger" value="danger" label="Danger"></el-option>
              </el-select>

              <br /><br /><br />

              <el-select v-model="configButton.column" class="select-success" placeholder="Select Column Width"
                style="width: 100%;">
                <el-option class="text-dark" value="col-3" label="col-3"></el-option>
                <el-option class="text-dark" value="col-4" label="col-4"></el-option>
                <el-option class="text-dark" value="col-5" label="col-5"></el-option>
                <el-option class="text-dark" value="col-6" label="col-6"></el-option>
                <el-option class="text-dark" value="col-7" label="col-7"></el-option>
                <el-option class="text-dark" value="col-8" label="col-8"></el-option>
                <el-option class="text-dark" value="col-9" label="col-9"></el-option>
                <el-option class="text-dark" value="col-10" label="col-10"></el-option>
                <el-option class="text-dark" value="col-11" label="col-11"></el-option>
                <el-option class="text-dark" value="col-12" label="col-12"></el-option>
              </el-select>

              <br /><br />
            </div>

            <!-- FORM SEND NUMBER TYPE -->
            <div v-if="selectedWidget.widget === 'sendNumber' || widgetType === 'sendNumber'">
              <base-input v-model="sendNumberConfig.variableFullName" label="Nombre" type="text">
              </base-input>

              <br />
              <base-input v-model="sendNumberConfig.message" label="Mensaje" type="text">
              </base-input>

              <br />

              <p>Valor del Mensaje</p>
              <el-select v-model="sendNumberConfig.messageValue" label="Mensaje a Enviar" class="select-success"
                placeholder="Seleccione un Valor" style="width: 100%;">
                <el-option class="text-success" value="1" label="1"></el-option>
                <el-option class="text-primary" value="2" label="2"></el-option>
                <el-option class="text-warning" value="3" label="3"></el-option>
                <el-option class="text-danger" value="4" label="4"></el-option>
                <el-option class="text-danger" value="5" label="5"></el-option>
                <el-option class="text-danger" value="6" label="6"></el-option>
                <el-option class="text-danger" value="7" label="7"></el-option>
                <el-option class="text-danger" value="8" label="8"></el-option>
                <el-option class="text-danger" value="9" label="9"></el-option>
                <el-option class="text-danger" value="10" label="10"></el-option>
                <el-option class="text-danger" value="11" label="11"></el-option>
                <el-option class="text-danger" value="12" label="12"></el-option>
                <el-option class="text-danger" value="13" label="13"></el-option>
                <el-option class="text-danger" value="14" label="14"></el-option>
                <el-option class="text-danger" value="15" label="15"></el-option>

              </el-select>

              <base-input v-model="sendNumberConfig.text" label="Texto del Boton" type="text">
              </base-input>

              <p>Selecciona el Icono</p>
              <el-select class="select-success" v-model="sendNumberConfig.icon"
                placeholder="Selecciona o ingresa un Icono" filterable @change="" style="width: 100%;">

                <el-option v-for="option in iconsList" :value="option.value" :label="option.label" :key="option.label">
                  <!-- Slot personalizado para mostrar el icono y el label -->
                  <span style="display: flex; align-items: center;">
                    <i :class="option.value" style="margin-right: 10px;"></i>
                    <span>{{ option.label }}</span>
                  </span>
                </el-option>

              </el-select>
              <br />

              <el-select v-model="sendNumberConfig.class" class="select-success" placeholder="Seleccione Clase"
                style="width: 100%;">
                <el-option class="text-success" value="success" label="Success"></el-option>
                <el-option class="text-primary" value="primary" label="Primary"></el-option>
                <el-option class="text-warning" value="warning" label="Warning"></el-option>
                <el-option class="text-danger" value="danger" label="Danger"></el-option>
              </el-select>

              <br /><br /><br />

              <el-select v-model="sendNumberConfig.column" class="select-success"
                placeholder="Seleccione ancho de columna" style="width: 100%;">
                <el-option class="text-dark" value="col-3" label="col-3"></el-option>
                <el-option class="text-dark" value="col-4" label="col-4"></el-option>
                <el-option class="text-dark" value="col-5" label="col-5"></el-option>
                <el-option class="text-dark" value="col-6" label="col-6"></el-option>
                <el-option class="text-dark" value="col-7" label="col-7"></el-option>
                <el-option class="text-dark" value="col-8" label="col-8"></el-option>
                <el-option class="text-dark" value="col-9" label="col-9"></el-option>
                <el-option class="text-dark" value="col-10" label="col-10"></el-option>
                <el-option class="text-dark" value="col-11" label="col-11"></el-option>
                <el-option class="text-dark" value="col-12" label="col-12"></el-option>
              </el-select>

              <br /><br />
            </div>

            <!-- FORM INDICATOR TYPE -->
            <div v-if="selectedWidget.widget === 'indicator' || widgetType === 'indicator'">

              <base-input v-model="iotIndicatorConfig.variableFullName" label="Nombre" type="text">
              </base-input>

              <p>Selecciona el Icono</p>
              <el-select class="select-success" v-model="iotIndicatorConfig.icon"
                placeholder="Selecciona o ingresa un Icono" filterable @change="" style="width: 100%;">

                <el-option v-for="option in iconsList" :value="option.value" :label="option.label" :key="option.label">
                  <!-- Slot personalizado para mostrar el icono y el label -->
                  <span style="display: flex; align-items: center;">
                    <i :class="option.value" style="margin-right: 10px;"></i>
                    <span>{{ option.label }}</span>
                  </span>
                </el-option>

              </el-select>
              <br />

              <base-input v-model="iotIndicatorConfig.variableSendFreq" label="Frecuencia de Envío"
                type="text"></base-input>

              <br />

              <el-select v-model="iotIndicatorConfig.class" class="select-success" placeholder="Select Class"
                style="width: 100%;">
                <el-option class="text-success" value="success" label="Success"></el-option>
                <el-option class="text-primary" value="primary" label="Primary"></el-option>
                <el-option class="text-warning" value="warning" label="Warning"></el-option>
                <el-option class="text-danger" value="danger" label="Danger"></el-option>
              </el-select>

              <br /><br /><br />

              <el-select v-model="iotIndicatorConfig.column" class="select-success" placeholder="Select Column Width"
                style="width: 100%;">
                <el-option class="text-dark" value="col-2" label="col-2"></el-option>
                <el-option class="text-dark" value="col-3" label="col-3"></el-option>
                <el-option class="text-dark" value="col-4" label="col-4"></el-option>
                <el-option class="text-dark" value="col-5" label="col-5"></el-option>
                <el-option class="text-dark" value="col-6" label="col-6"></el-option>
                <el-option class="text-dark" value="col-7" label="col-7"></el-option>
                <el-option class="text-dark" value="col-8" label="col-8"></el-option>
                <el-option class="text-dark" value="col-9" label="col-9"></el-option>
                <el-option class="text-dark" value="col-10" label="col-10"></el-option>
                <el-option class="text-dark" value="col-11" label="col-11"></el-option>
                <el-option class="text-dark" value="col-12" label="col-12"></el-option>
              </el-select>

              <br /><br />
            </div>

            <!-- FORM MAP TYPE -->
            <div v-if="selectedWidget.widget === 'map' || widgetType === 'map'">

              <base-input v-model="iotMapConfig.variableFullName" label="Nombre" type="text">
              </base-input>

              <p>Selecciona el Icono</p>
              <el-select class="select-success" v-model="iotMapConfig.icon" placeholder="Selecciona o ingresa un Icono"
                filterable @change="" style="width: 100%;">

                <el-option v-for="option in iconsList" :value="option.value" :label="option.label" :key="option.label">
                  <!-- Slot personalizado para mostrar el icono y el label -->
                  <span style="display: flex; align-items: center;">
                    <i :class="option.value" style="margin-right: 10px;"></i>
                    <span>{{ option.label }}</span>
                  </span>
                </el-option>

              </el-select>
              <br />

              <base-input v-model="iotMapConfig.variableSendFreq" label="Frecuencia de Envío" type="text"></base-input>

              <br />


              <el-select v-model="iotMapConfig.class" class="select-success" placeholder="Select Class"
                style="width: 100%;">
                <el-option class="text-success" value="success" label="Success"></el-option>
                <el-option class="text-primary" value="primary" label="Primary"></el-option>
                <el-option class="text-warning" value="warning" label="Warning"></el-option>
                <el-option class="text-danger" value="danger" label="Danger"></el-option>
              </el-select>

              <br /><br /><br />

              <el-select v-model="iotMapConfig.column" class="select-success" placeholder="Select Column Width"
                style="width: 100%;">
                <el-option class="text-dark" value="col-2" label="col-2"></el-option>
                <el-option class="text-dark" value="col-3" label="col-3"></el-option>
                <el-option class="text-dark" value="col-4" label="col-4"></el-option>
                <el-option class="text-dark" value="col-5" label="col-5"></el-option>
                <el-option class="text-dark" value="col-6" label="col-6"></el-option>
                <el-option class="text-dark" value="col-7" label="col-7"></el-option>
                <el-option class="text-dark" value="col-8" label="col-8"></el-option>
                <el-option class="text-dark" value="col-9" label="col-9"></el-option>
                <el-option class="text-dark" value="col-10" label="col-10"></el-option>
                <el-option class="text-dark" value="col-11" label="col-11"></el-option>
                <el-option class="text-dark" value="col-12" label="col-12"></el-option>
              </el-select>

              <br /><br />
            </div>

          </div>

          <!-- WIDGET PREVIEW -->
          <div class="col-6">
            <Rtnumberchart v-if="selectedWidget.widget === 'numberchart' || widgetType === 'numberchart'"
              :config="ncConfig"></Rtnumberchart>
            <Iotswitch v-if="selectedWidget.widget === 'switch' || widgetType === 'switch'" :config="iotSwitchConfig">
            </Iotswitch>
            <Iotbutton v-if="selectedWidget.widget === 'button' || widgetType === 'button'" :config="configButton">
            </Iotbutton>
            <Iotindicator v-if="selectedWidget.widget === 'indicator' || widgetType === 'indicator'"
              :config="iotIndicatorConfig"></Iotindicator>
            <IotMap v-if="selectedWidget.widget === 'map' || widgetType === 'map'" :config="iotMapConfig"></IotMap>
            <IotSendNumber v-if="selectedWidget.widget === 'sendNumber' || widgetType === 'sendNumber'"
              :config="sendNumberConfig"></IotSendNumber>

          </div>


          <br><br><br>
        </div>

        <!-- ADD WIDGET BUTTON -->
        <div class="row pull-right" v-if="!templateEdition && !mostrarFormularioAddWidget">
          <div class="col-12">
            <base-button native-type="submit" type="primary" class="mb-3" size="lg" @click="addNewWidget()">
              Crear Widget
            </base-button>
          </div>
        </div>

        <!-- UPDATE WIDGET BUTTON -->
        <div class="row " v-if="templateEdition && !mostrarFormularioAddWidget">

          <div class="col-6">

            <base-button native-type="submit" type="success" class="mb-3 animation-on-hover " size="lg"
              @click="mostrarFormularioAddWidget = true, selectedWidget = ''"
              :disabled="!selectedTemplate.name || selectedTemplate.name.trim() === ''">
              Añadir Nuevo Widget
            </base-button>
          </div>

          <div class="col-6 pull-right">


            <base-button native-type="submit" type="primary" class="mb-3 animation-on-hover" size="lg"
              @click="updateWidget(selectedWidget)"
              :disabled="!selectedWidget.variable || selectedWidget.variable.trim() === ''">
              Actualizar Widget
            </base-button>

            <base-button class="mb-3 pull-right animation-on-hover" type="default" size="lg" @click="editCancel()">
              Cancelar Actualizacion
            </base-button>

          </div>
        </div>

        <!-- EDIT ADD WIDGET BUTTON -->
        <div class="row  pull-right" v-if="mostrarFormularioAddWidget">

          <base-button native-type="submit" type="success" class="mb-3 animation-on-hover" size="lg"
            @click="addNewWidget()" :disabled="!widgetType || widgetType.trim() === ''">
            Agregar Widget
          </base-button>

          <base-button class="mb-3 animation-on-hover" type="default" size="lg"
            @click="mostrarFormularioAddWidget = false, widgetType = ''">
            Cancelar
          </base-button>

        </div>


      </card>
    </div>

    <div class="row" ref="selectedTemplate">
      <card>


        <!-- Vista previa del template con los widgets -->

        <div class="row" v-if="selectedTemplate && templateEdition">
          <div class="row col-12">
            <h2 class="text-center">Preview de la plantilla seleccionada para edición</h2>
          </div>

          <div class="container">
            <div class="row col-12">
              <draggable v-model="selectedTemplate.widgets" @end="">
                <div v-for="(widget, index) in selectedTemplate.widgets" :key="widget.variable" class="widget-card">
                  <span class="btn btn-lg animation-on-hover drag-handle">☰ {{ widget.variableFullName }}</span>
                  <!-- Icono para arrastrar -->
                </div>
              </draggable>
            </div>
          </div>

          <div class="row col-12">
            <div v-for="(widget, Index) in selectedTemplate.widgets" :key="widget.variable" :class="[widget.column]">

              <i class="fa fa-trash text-warning pull-right" @click="deleteWidget(Index)"
                style="margin-bottom: 10px;"></i>
              <i class="fa fa-edit text-primary pull-right" @click="editWidget(selectedTemplate, widget)"
                style="margin-bottom: 10px; margin-right: 10px;"></i>

              <Rtnumberchart v-if="widget.widget == 'numberchart'" :config="widget"></Rtnumberchart>
              <Iotswitch v-if="widget.widget == 'switch'" :config="widget"></Iotswitch>
              <Iotbutton v-if="widget.widget == 'button'" :config="widget"></Iotbutton>
              <Iotindicator v-if="widget.widget == 'indicator'" :config="widget"></Iotindicator>
              <IotMap v-if="widget.widget == 'map'" :config="widget"></IotMap>
              <IotSendNumber v-if="widget.widget == 'sendNumber'" :config="widget"></IotSendNumber>
            </div>

          </div>
        </div>
        <p v-else>Crea un Widget para ver el Preview</p>




        <!-- DASHBOARD PREVIEW -->
        <div class="row" v-if="!templateEdition">
          <div v-for="(widget, index) in widgets" :key="index" :class="[widget.column]">
            <i aria-hidden="true" class="fa fa-trash text-warning pull-right" @click="deleteWidget(index)"
              style="margin-bottom: 10px;"></i>

            <Rtnumberchart v-if="widget.widget == 'numberchart'" :config="widget"></Rtnumberchart>

            <Iotswitch v-if="widget.widget == 'switch'" :config="widget"></Iotswitch>

            <Iotbutton v-if="widget.widget == 'button'" :config="widget"></Iotbutton>

            <Iotindicator v-if="widget.widget == 'indicator'" :config="widget"></Iotindicator>

            <IotMap v-if="widget.widget == 'map'" :config="widget"></IotMap>

            <IotSendNumber v-if="widget.widget == 'sendNumber'" :config="widget"></IotSendNumber>

          </div>
        </div>
      </card>
    </div>

    <!-- SAVE TEMPLATE FORM-->
    <div class="row">
      <card>
        <div slot="header">
          <h4 v-if="!templateEdition" class="card-title">Guardar Plantilla</h4>
          <h4 v-else class="card-title">Actualizar Plantilla</h4>

        </div>

        <div class="row">
          <base-input class="col-4" v-model="templateName" label="Nombre de la plantilla" type="text">
          </base-input>

          <base-input class="col-8" v-model="templateDescription" label="Descripcion de la plantilla" type="text">
          </base-input>
        </div>

        <br /><br />

        <div class="row" v-if="!templateEdition">
          <div class="col-12">
            <base-button native-type="submit" type="primary" class="mb-3 pull-right" size="lg" @click="saveTemplate()"
              :disabled="widgets.length == 0">
              Guardar Plantilla
            </base-button>
          </div>
        </div>

        <div class="row" v-if="templateEdition">
          <div class="col-12">

            <base-button class="mb-3 pull-right animation-on-hover" type="default" size="lg" @click="editCancel()">
              Cancelar
            </base-button>

            <base-button native-type="submit" type="primary" class="mb-3 pull-right animation-on-hover" size="lg"
              @click="updateTemplate(selectedTemplate, templateName, templateDescription)"
              :disabled="!selectedTemplate.name || selectedTemplate.name.trim() === ''">
              Actualizar Plantilla
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

            <el-table-column prop="description" label="Description"></el-table-column>

            <el-table-column prop="widgets.length" label="Widgets"></el-table-column>

            <el-table-column header-align="right" align="right" label="Actions">
              <div slot-scope="{ row, $index }" class="text-right table-actions">

                <el-tooltip content="Edit" effect="light" :open-delay="300" placement="top">
                  <base-button @click="editTemplate(row, $index)" type="success" icon size="sm">
                    <i class="fa fa-edit"></i>
                  </base-button>
                </el-tooltip>


                <el-tooltip content="Delete" effect="light" :open-delay="300" placement="top">
                  <base-button @click="deleteTemplate(row)" type="danger" icon size="sm" class="btn-link">
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

      iconsList: [
        { value: 'fa fa-sun', label: 'Sol | Sun' },
        { value: 'fa fa-moon', label: 'Luna | Moon' },
        { value: 'fa fa-star', label: 'Estrella | Star' },
        { value: 'fa fa-cloud', label: 'Nube | Cloud' },
        { value: 'fa fa-heart', label: 'Corazón | Heart' },
        { value: 'fa fa-fire', label: 'Fuego | Fire' },
        { value: 'fa fa-home', label: 'Casa | Home' },
        { value: 'fa fa-envelope', label: 'Sobre | Envelope' },
        { value: 'fa fa-bell', label: 'Campana | Bell' },
        { value: 'fa fa-cog', label: 'Engranaje | Cog' },
        { value: 'fa fa-user', label: 'Usuario | User' },
        { value: 'fa fa-users', label: 'Usuarios | Users' },
        { value: 'fa fa-search', label: 'Buscar | Search' },
        { value: 'fa fa-lock', label: 'Candado | Lock' },
        { value: 'fa fa-unlock', label: 'Candado abierto | Unlock' },
        { value: 'fa fa-camera', label: 'Cámara | Camera' },
        { value: 'fa fa-image', label: 'Imagen | Image' },
        { value: 'fa fa-film', label: 'Película | Film' },
        { value: 'fa fa-music', label: 'Música | Music' },
        { value: 'fa fa-book', label: 'Libro | Book' },
        { value: 'fa fa-flag', label: 'Bandera | Flag' },
        { value: 'fa fa-globe', label: 'Globo | Globe' },
        { value: 'fa fa-trash', label: 'Basura | Trash' },
        { value: 'fa fa-download', label: 'Descargar | Download' },
        { value: 'fa fa-upload', label: 'Subir | Upload' },
        { value: 'fa fa-print', label: 'Imprimir | Print' },
        { value: 'fa fa-save', label: 'Guardar | Save' },
        { value: 'fa fa-edit', label: 'Editar | Edit' },
        { value: 'fa fa-share', label: 'Compartir | Share' },
        { value: 'fa fa-thumbs-up', label: 'Pulgar arriba | Thumbs Up' },
        { value: 'fa fa-thumbs-down', label: 'Pulgar abajo | Thumbs Down' },
        { value: 'fa fa-smile', label: 'Sonreír | Smile' },
        { value: 'fa fa-frown', label: 'Fruncir el ceño | Frown' },
        { value: 'fa fa-calendar', label: 'Calendario | Calendar' },
        { value: 'fa fa-clock', label: 'Reloj | Clock' },
        { value: 'fa fa-map-marker', label: 'Marcador de mapa | Map Marker' },
        { value: 'fa fa-phone', label: 'Teléfono | Phone' },
        { value: 'fa fa-link', label: 'Enlace | Link' },
        { value: 'fa fa-paperclip', label: 'Clip | Paperclip' },
        { value: 'fa fa-folder', label: 'Carpeta | Folder' },
        { value: 'fa fa-folder-open', label: 'Carpeta abierta | Folder Open' },
        { value: 'fa fa-desktop', label: 'Escritorio | Desktop' },
        { value: 'fa fa-laptop', label: 'Portátil | Laptop' },
        { value: 'fa fa-tablet', label: 'Tableta | Tablet' },
        { value: 'fa fa-mobile', label: 'Móvil | Mobile' },
        { value: 'fa fa-key', label: 'Llave | Key' },
        { value: 'fa fa-battery-full', label: 'Batería llena | Battery Full' },
        { value: 'fa fa-battery-half', label: 'Batería a la mitad | Battery Half' },
        { value: 'fa fa-battery-empty', label: 'Batería vacía | Battery Empty' },
        { value: 'fa fa-wifi', label: 'WiFi | WiFi' },
        { value: 'fa fa-bluetooth', label: 'Bluetooth | Bluetooth' },
        { value: 'fa fa-credit-card', label: 'Tarjeta de crédito | Credit Card' },
        { value: 'fa fa-paypal', label: 'PayPal | PayPal' },
        { value: 'fa fa-bitcoin', label: 'Bitcoin | Bitcoin' },
        { value: 'fa fa-car', label: 'Coche | Car' },
        { value: 'fa fa-bus', label: 'Autobús | Bus' },
        { value: 'fa fa-train', label: 'Tren | Train' },
        { value: 'fa fa-plane', label: 'Avión | Plane' },
        { value: 'fa fa-ship', label: 'Barco | Ship' },
        { value: 'fa fa-bicycle', label: 'Bicicleta | Bicycle' },
        { value: 'fa fa-walking', label: 'Caminar | Walking' },
        { value: 'fa fa-running', label: 'Correr | Running' },
        { value: 'fa fa-medkit', label: 'Botiquín | Medkit' },
        { value: 'fa fa-stethoscope', label: 'Estetoscopio | Stethoscope' },
        { value: 'fa fa-ambulance', label: 'Ambulancia | Ambulance' },
        { value: 'fa fa-paw', label: 'Pata | Paw' },
        { value: 'fa fa-leaf', label: 'Hoja | Leaf' },
        { value: 'fa fa-tree', label: 'Árbol | Tree' },
        { value: 'fa fa-tint', label: 'Gota | Tint' },
        { value: 'fa fa-snowflake', label: 'Copo de nieve | Snowflake' },
        { value: 'fa fa-umbrella', label: 'Paraguas | Umbrella' },
        { value: 'fa fa-gift', label: 'Regalo | Gift' },
        { value: 'fa fa-birthday-cake', label: 'Pastel de cumpleaños | Birthday Cake' },
        { value: 'fa fa-glass-cheers', label: 'Brindis | Glass Cheers' },
        { value: 'fa fa-rocket', label: 'Cohete | Rocket' },
        { value: 'fa fa-microchip', label: 'Microchip | Microchip' },
        { value: 'fa fa-robot', label: 'Robot | Robot' },
        { value: 'fa fa-gamepad', label: 'Mando de juego | Gamepad' },
        { value: 'fa fa-dice', label: 'Dado | Dice' },
        { value: 'fa fa-chess', label: 'Ajedrez | Chess' },
        { value: 'fa fa-puzzle-piece', label: 'Pieza de rompecabezas | Puzzle Piece' },
        { value: 'fa fa-paint-brush', label: 'Pincel | Paint Brush' },
        { value: 'fa fa-palette', label: 'Paleta | Palette' },
        { value: 'fa fa-music', label: 'Música | Music' },
        { value: 'fa fa-headphones', label: 'Auriculares | Headphones' },
        { value: 'fa fa-video', label: 'Vídeo | Video' },
        { value: 'fa fa-film', label: 'Película | Film' },
        { value: 'fa fa-camera', label: 'Cámara | Camera' },
        { value: 'fa fa-image', label: 'Imagen | Image' },
        { value: 'fa fa-photo-video', label: 'Foto/Vídeo | Photo Video' },
        { value: 'fa fa-microphone', label: 'Micrófono | Microphone' },
        { value: 'fa fa-volume-up', label: 'Volumen alto | Volume Up' },
        { value: 'fa fa-volume-down', label: 'Volumen bajo | Volume Down' },
        { value: 'fa fa-volume-off', label: 'Silenciar | Volume Off' },
        { value: 'fa fa-comment', label: 'Comentario | Comment' },
        { value: 'fa fa-comments', label: 'Comentarios | Comments' },
        { value: 'fa fa-envelope', label: 'Sobre | Envelope' },
        { value: 'fa fa-paper-plane', label: 'Avión de papel | Paper Plane' },
        { value: 'fa fa-inbox', label: 'Bandeja de entrada | Inbox' },
        { value: 'fa fa-archive', label: 'Archivo | Archive' },
        { value: 'fa fa-trash', label: 'Basura | Trash' },
        { value: 'fa fa-folder', label: 'Carpeta | Folder' },
        { value: 'fa fa-folder-open', label: 'Carpeta abierta | Folder Open' },
        { value: 'fa fa-tag', label: 'Etiqueta | Tag' },
        { value: 'fa fa-tags', label: 'Etiquetas | Tags' },
        { value: 'fa fa-bookmark', label: 'Marcador | Bookmark' },
        { value: 'fa fa-flag', label: 'Bandera | Flag' },
        { value: 'fa fa-share', label: 'Compartir | Share' },
        { value: 'fa fa-share-alt', label: 'Compartir alternativo | Share Alt' },
        { value: 'fa fa-retweet', label: 'Retweet | Retweet' },
        { value: 'fa fa-thumbs-up', label: 'Pulgar arriba | Thumbs Up' },
        { value: 'fa fa-thumbs-down', label: 'Pulgar abajo | Thumbs Down' },
        { value: 'fa fa-heart', label: 'Corazón | Heart' },
        { value: 'fa fa-smile', label: 'Sonreír | Smile' },
        { value: 'fa fa-frown', label: 'Fruncir el ceño | Frown' },
        { value: 'fa fa-meh', label: 'Meh | Meh' },
        { value: 'fa fa-star', label: 'Estrella | Star' },
        { value: 'fa fa-star-half', label: 'Media estrella | Star Half' },
        { value: 'fa fa-moon', label: 'Luna | Moon' },
        { value: 'fa fa-sun', label: 'Sol | Sun' },
        { value: 'fa fa-cloud', label: 'Nube | Cloud' },
        { value: 'fa fa-umbrella', label: 'Paraguas | Umbrella' },
        { value: 'fa fa-snowflake', label: 'Copo de nieve | Snowflake' },
        { value: 'fa fa-fire', label: 'Fuego | Fire' },
        { value: 'fa fa-bolt', label: 'Rayo | Bolt' },
        { value: 'fa fa-tint', label: 'Gota | Tint' },
        { value: 'fa fa-leaf', label: 'Hoja | Leaf' },
        { value: 'fa fa-tree', label: 'Árbol | Tree' },
        { value: 'fa fa-seedling', label: 'Plántula | Seedling' },
        { value: 'fa fa-dog', label: 'Perro | Dog' },
        { value: 'fa fa-cat', label: 'Gato | Cat' },
        { value: 'fa fa-horse', label: 'Caballo | Horse' },
        { value: 'fa fa-fish', label: 'Pez | Fish' },
        { value: 'fa fa-kiwi-bird', label: 'Kiwi | Kiwi Bird' },
        { value: 'fa fa-spider', label: 'Araña | Spider' },
        { value: 'fa fa-bug', label: 'Bicho | Bug' },
        { value: 'fa fa-biohazard', label: 'Biohazard | Biohazard' },
        { value: 'fa fa-radiation', label: 'Radiación | Radiation' },
        { value: 'fa fa-skull-crossbones', label: 'Calavera y huesos | Skull Crossbones' },
        { value: 'fa fa-poop', label: 'Caca | Poop' },
        { value: 'fa fa-ghost', label: 'Fantasma | Ghost' },
        { value: 'fa fa-robot', label: 'Robot | Robot' },
        { value: 'fa fa-microchip', label: 'Microchip | Microchip' },
        { value: 'fa fa-gamepad', label: 'Mando de juego | Gamepad' },
        { value: 'fa fa-dice', label: 'Dado | Dice' },
        { value: 'fa fa-chess', label: 'Ajedrez | Chess' },
        { value: 'fa fa-puzzle-piece', label: 'Pieza de rompecabezas | Puzzle Piece' },
        { value: 'fa fa-paint-brush', label: 'Pincel | Paint Brush' },
        { value: 'fa fa-palette', label: 'Paleta | Palette' },
        { value: 'fa fa-music', label: 'Música | Music' },
        { value: 'fa fa-headphones', label: 'Auriculares | Headphones' },
        { value: 'fa fa-video', label: 'Vídeo | Video' },
        { value: 'fa fa-film', label: 'Película | Film' },
        { value: 'fa fa-camera', label: 'Cámara | Camera' },
        { value: 'fa fa-image', label: 'Imagen | Image' },
        { value: 'fa fa-photo-video', label: 'Foto/Vídeo | Photo Video' },
        { value: 'fa fa-microphone', label: 'Micrófono | Microphone' },
        { value: 'fa fa-volume-up', label: 'Volumen alto | Volume Up' },
        { value: 'fa fa-volume-down', label: 'Volumen bajo | Volume Down' },
        { value: 'fa fa-volume-off', label: 'Silenciar | Volume Off' },
        { value: 'fa fa-comment', label: 'Comentario | Comment' },
        { value: 'fa fa-comments', label: 'Comentarios | Comments' },
        { value: 'fa fa-envelope', label: 'Sobre | Envelope' },
        { value: 'fa fa-paper-plane', label: 'Avión de papel | Paper Plane' },
        { value: 'fa fa-inbox', label: 'Bandeja de entrada | Inbox' },
        { value: 'fa fa-archive', label: 'Archivo | Archive' },
        { value: 'fa fa-trash', label: 'Basura | Trash' },
        { value: 'fa fa-folder', label: 'Carpeta | Folder' },
        { value: 'fa fa-folder-open', label: 'Carpeta abierta | Folder Open' },
        { value: 'fa fa-tag', label: 'Etiqueta | Tag' },
        { value: 'fa fa-tags', label: 'Etiquetas | Tags' },
        { value: 'fa fa-bookmark', label: 'Marcador | Bookmark' },
        { value: 'fa fa-flag', label: 'Bandera | Flag' },
        { value: 'fa fa-share', label: 'Compartir | Share' },
        { value: 'fa fa-share-alt', label: 'Compartir alternativo | Share Alt' },
        { value: 'fa fa-retweet', label: 'Retweet | Retweet' },
        { value: 'fa fa-thumbs-up', label: 'Pulgar arriba | Thumbs Up' },
        { value: 'fa fa-thumbs-down', label: 'Pulgar abajo | Thumbs Down' },
        { value: 'fa fa-heart', label: 'Corazón | Heart' },
        { value: 'fa fa-smile', label: 'Sonreír | Smile' },
        { value: 'fa fa-frown', label: 'Fruncir el ceño | Frown' },
        { value: 'fa fa-meh', label: 'Meh | Meh' },
        { value: 'fa fa-star', label: 'Estrella | Star' },
        { value: 'fa fa-star-half', label: 'Media estrella | Star Half' },
        { value: 'fa fa-moon', label: 'Luna | Moon' },
        { value: 'fa fa-sun', label: 'Sol | Sun' },
        { value: 'fa fa-cloud', label: 'Nube | Cloud' },
        { value: 'fa fa-umbrella', label: 'Paraguas | Umbrella' },
        { value: 'fa fa-snowflake', label: 'Copo de nieve | Snowflake' },
        { value: 'fa fa-fire', label: 'Fuego | Fire' },
        { value: 'fa fa-bolt', label: 'Rayo | Bolt' },
        { value: 'fa fa-tint', label: 'Gota | Tint' },
        { value: 'fa fa-leaf', label: 'Hoja | Leaf' },
        { value: 'fa fa-tree', label: 'Árbol | Tree' },
        { value: 'fa fa-seedling', label: 'Plántula | Seedling' },
        { value: 'fa fa-paw', label: 'Pata | Paw' },
        { value: 'fa fa-dog', label: 'Perro | Dog' },
        { value: 'fa fa-cat', label: 'Gato | Cat' },
        { value: 'fa fa-horse', label: 'Caballo | Horse' },
        { value: 'fa fa-fish', label: 'Pez | Fish' },
        { value: 'fa fa-kiwi-bird', label: 'Kiwi | Kiwi Bird' },
        { value: 'fa fa-spider', label: 'Araña | Spider' },
        { value: 'fa fa-bug', label: 'Bicho | Bug' },
        { value: 'fa fa-biohazard' },
      ],


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
        column: "col-4"
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
        message: "{'pumpstatus': 'stop'}",
        text: "Start",
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
        variableFullName: "Motor",
        variable: "var1",
        variableType: "output",
        icon: "fa-caret-square-right",
        column: "col-4",
        widget: "sendNumber",
        class: "danger",
        message: "Tiempo en minutos",
        messageValue: 1,
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
          if (newVal.widget == "numberchart") {
            this.ncConfig.userId = newVal.userId,
              this.ncConfig.selectedDevice = {
                name: newVal.selectedDevice.name,
                dId: newVal.selectedDevice.dId
              },
              this.ncConfig.variableFullName = newVal.variableFullName,
              this.ncConfig.variable = newVal.variable,
              this.ncConfig.variableType = newVal.variableType,
              this.ncConfig.variableSendFreq = newVal.variableSendFreq,
              this.ncConfig.unit = newVal.unit,
              this.ncConfig.class = newVal.class,
              this.ncConfig.column = newVal.column,
              this.ncConfig.decimalPlaces = newVal.decimalPlaces,
              this.ncConfig.widget = newVal.widget,
              this.ncConfig.icon = newVal.icon,
              this.ncConfig.chartTimeAgo = newVal.chartTimeAgo,
              this.ncConfig.demo = newVal.demo
          }

          if (newVal.widget == "switch") {
            this.iotSwitchConfig.userId = newVal.userId,
              this.iotSwitchConfig.selectedDevice = {
                name: newVal.selectedDevice.name,
                dId: newVal.selectedDevice.dId
              },
              this.iotSwitchConfig.variableFullName = newVal.variableFullName,
              this.iotSwitchConfig.variable = newVal.variable,
              this.iotSwitchConfig.variableType = newVal.variableType,
              this.iotSwitchConfig.class = newVal.class,
              this.iotSwitchConfig.widget = newVal.widget,
              this.iotSwitchConfig.icon = newVal.icon,
              this.iotSwitchConfig.column = newVal.column
          }


          if (newVal.widget == "indicator") {
            this.iotIndicatorConfig.userId = newVal.userId,
              this.iotIndicatorConfig.selectedDevice = {
                name: newVal.selectedDevice.name,
                dId: newVal.selectedDevice.dId
              },
              this.iotIndicatorConfig.variableFullName = newVal.variableFullName,
              this.iotIndicatorConfig.variable = newVal.variable,
              this.iotIndicatorConfig.variableType = newVal.variableType,
              this.iotIndicatorConfig.variableSendFreq = newVal.variableSendFreq,
              this.iotIndicatorConfig.class = newVal.class,
              this.iotIndicatorConfig.widget = newVal.widget,
              this.iotIndicatorConfig.icon = newVal.icon,
              this.iotIndicatorConfig.column = newVal.column
          }

          if (newVal.widget == "button") {
            this.configButton.userId = newVal.userId,
              this.configButton.selectedDevice = {
                name: newVal.selectedDevice.name,
                dId: newVal.selectedDevice.dId,
                templateName: newVal.selectedDevice.templateName,
                templateId: newVal.selectedDevice.templateId,
                saverRule: newVal.selectedDevice.saverRule
              },
              this.configButton.variableFullName = newVal.variableFullName,
              this.configButton.variable = newVal.variable,
              this.configButton.variableType = newVal.variableType,
              this.configButton.icon = newVal.icon,
              this.configButton.column = newVal.column,
              this.configButton.widget = newVal.widget,
              this.configButton.class = newVal.class,
              this.configButton.message = newVal.message,
              this.configButton.text = newVal.text
          }

          if (newVal.widget == "sendNumber") {
            this.sendNumberConfig.userId = newVal.userId,
              this.sendNumberConfig.selectedDevice = {
                name: newVal.selectedDevice.name,
                dId: newVal.selectedDevice.dId,
                templateName: newVal.selectedDevice.templateName,
                templateId: newVal.selectedDevice.templateId,
                saverRule: newVal.selectedDevice.saverRule
              },
              this.sendNumberConfig.variableFullName = newVal.variableFullName,
              this.sendNumberConfig.variable = newVal.variable,
              this.sendNumberConfig.variableType = newVal.variableType,
              this.sendNumberConfig.icon = newVal.icon,
              this.sendNumberConfig.column = newVal.comumn,
              this.sendNumberConfig.widget = newVal.widget
            this.sendNumberConfig.class = newVal.class,
              this.sendNumberConfig.message = newVal.message,
              this.sendNumberConfig.messageValue = newVal.messageValue,
              this.sendNumberConfig.text = newVal.text

          }

          if (newVal.widget == "map") {
            this.iotMapConfig.userId = newVal.userId,
              this.iotMapConfig.selectedDevice = {
                name: newVal.selectedDevice.name,
                dId: newVal.selectedDevice.dId
              },
              this.iotMapConfig.variableFullName = newVal.variableFullName,
              this.iotMapConfig.variable = newVal.variable,
              this.iotMapConfig.variableType = newVal.variableType,
              this.iotMapConfig.variableSendFreq = newVal.variableSendFreq,
              this.iotMapConfig.class = newVal.class,
              this.iotMapConfig.widget = newVal.widget,
              this.iotMapConfig.icon = newVal.icon,
              this.iotMapConfig.column = newVal.column
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
  },

  methods: {
    // Editar widget - Carga datos en selectedTemplate y selectedWidget, activa edición
    editWidget(selectedTemplate, widget) {
      this.widgetType = "";
      this.mostrarFormularioAddWidget = false;
      this.selectedTemplate = { ...selectedTemplate }; // Clonamos el objeto
      this.selectedWidget = widget;
      this.templateEdition = true;


      // Hacer scroll al inicio de la página
      window.scrollTo({
        top: 0,
        behavior: "smooth"
      });


      // Hacer scroll al div donde se edita el widget
      this.$refs.inicio.scrollIntoView({ behavior: "smooth" });

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
      //Hacer scroll al inicio de la página
      window.scrollTo({
        top: 0,
        behavior: "smooth"
      });

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
        params: {
          templateId: template._id
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
        if (this.templateEdition) {
          console.log(this.selectedTemplate);

          console.log(this.ncConfig);

          this.selectedTemplate.widgets.push(JSON.parse(JSON.stringify(this.ncConfig)));
        } else {
          this.widgets.push(JSON.parse(JSON.stringify(this.ncConfig)));
        }
      }

      if (this.widgetType == "switch") {
        this.iotSwitchConfig.variable = this.makeid(10);
        if (this.templateEdition) {
          this.selectedTemplate.widgets.push(JSON.parse(JSON.stringify(this.iotSwitchConfig)));
        } else {
          this.widgets.push(JSON.parse(JSON.stringify(this.iotSwitchConfig)));
        }
      }

      if (this.widgetType == "button") {
        this.configButton.variable = this.makeid(10);

        if (this.templateEdition) {
          this.selectedTemplate.widgets.push(JSON.parse(JSON.stringify(this.configButton)));
        } else {
          this.widgets.push(JSON.parse(JSON.stringify(this.configButton)));
        }
      }

      if (this.widgetType == "indicator") {
        this.iotIndicatorConfig.variable = this.makeid(10);

        if (this.templateEdition) {
          this.selectedTemplate.widgets.push(JSON.parse(JSON.stringify(this.iotIndicatorConfig)));
        } else {
          this.widgets.push(JSON.parse(JSON.stringify(this.iotIndicatorConfig)));
        }
      }

      if (this.widgetType == "map") {
        this.iotMapConfig.variable = this.makeid(10);

        if (this.templateEdition) {
          this.selectedTemplate.widgets.push(JSON.parse(JSON.stringify(this.iotMapConfig)));
        } else {
          this.widgets.push(JSON.parse(JSON.stringify(this.iotMapConfig)));
        }
      }

      if (this.widgetType == "sendNumber") {
        this.sendNumberConfig.variable = this.makeid(10);

        if (this.templateEdition) {
          this.selectedTemplate.widgets.push(JSON.parse(JSON.stringify(this.sendNumberConfig)));
        } else {
          this.widgets.push(JSON.parse(JSON.stringify(this.sendNumberConfig)));
        }
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

          if (this.templateEdition) {
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
          this.templateEdition = false;
          this.selectedTemplate = {};
          this.selectedDevice = {};
          this.mostrarFormularioAddWidget = false;
          this.templateName = "";
          this.templateDescription = "";

          //Hacer scroll al inicio de la página
          window.scrollTo({
            top: 0,
            behavior: "smooth"
          });

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
