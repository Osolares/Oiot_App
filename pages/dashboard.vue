<template>


    
  <div class="row" v-if="$store.state.devices.length > 0">

    <div
      v-for="(widget, index) in $store.state.selectedDevice.template.widgets"
      :key="index"
      :class="[widget.column]"
      
    >
      <Rtnumberchart
        v-if="widget.widget == 'numberchart'"
        :config="fixWidget(widget)"
      ></Rtnumberchart>

      <Iotswitch
        v-if="widget.widget == 'switch'"
        :config="fixWidget(widget)"
      ></Iotswitch>

      <Iotbutton
        v-if="widget.widget == 'button'"
        :config="fixWidget(widget)"
      ></Iotbutton>

      <Iotindicator
        v-if="widget.widget == 'indicator'"
        :config="fixWidget(widget)"
      ></Iotindicator>

      <IotMap
        v-if="widget.widget == 'map'"
        :config="fixWidget(widget)"
      ></IotMap>

      <IotSendNumber
        v-if="widget.widget == 'sendNumber'"
        :config="fixWidget(widget)"
      ></IotSendNumber>

    </div>
  </div>

  <div v-else>
    Selecciona un Dispositivo...
  </div>

</template>
<script>

export default {
  middleware: 'authenticated',
  name: 'Dashboard',
  data() {
    return {


    } 
  },

  mounted() {
  

  },

  methods: {

    fixWidget(widget){
      var widgetCopy = JSON.parse(JSON.stringify(widget));
      widgetCopy.selectedDevice.dId = this.$store.state.selectedDevice.dId;
      widgetCopy.selectedDevice.name = this.$store.state.selectedDevice.name;
      widgetCopy.userId = this.$store.state.selectedDevice.userId;

      if (widget.widget =="numberchart"){
        widgetCopy.demo = false;
      }
      
      return widgetCopy;
    }

  }

};
</script>