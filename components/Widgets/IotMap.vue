<template>
  <card>
    <div slot="header">
      <h4 class="card-title">
        {{ config.selectedDevice.name }} - {{ config.variableFullName }}
      </h4>
    </div>

    <i
      class="fa "
      :class="[config.icon, getIconColorClass()]"
      style="font-size: 30px"
    ></i>

    <GmapMap 
        style="width: 100%; height: 400px;"
        :zoom="15"
        :center="{lat:14.564976132643977,lng:-90.46595274075851}"
        ref="map"
        >

      <GmapMarker
        :position="position"
        :clickable="true"
        :draggable="false"
        >
      </GmapMarker>


    </GmapMap>

  </card>
</template>

<script>
export default {
  name: "mapa",
  props: ['config'],
  data() {
    return {
      position:{lat:14.564976132643977,lng:-90.46595274075851},
      topic: "",
      props: ['config']      
    };
  },
  watch:  {
            config: {
                immediate: true,
                deep: true,
                handler() {
                    setTimeout(() => {
                        this.value = false;

                        this.$nuxt.$off(this.topic);

                        //userId/dId/uniquestr/sdata
                        const topic = this.config.userId + "/" + this.config.selectedDevice.dId + "/" + this.config.variable + "/sdata";
                        this.$nuxt.$on(topic, this.processReceivedData);

                    }, 300);
                }
            }
        },
  mounted(){
    const topic = this.config.userId + "/" + this.config.selectedDevice.dId + "/" + this.config.variable + "/sdata";
    this.$nuxt.$on(topic, this.processReceivedData);
  },
  beforeDestroy(){
    this.$nuxt.$off(this.topic); 
  },
  methods: {

    processReceivedData(data){
      try {
        console.log("Recibiendo Ubicacion");
        console.log(data);
        this.position = data;
      } catch (error) {
        console.log(error);
      }
    },
      
    getIconColorClass() {
      if (!this.position) {
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
    }

  }
};


</script>
