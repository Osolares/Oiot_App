<template>
    <div>
        <GmapMap 
        style="width: 100%; height: 450px;"
        :zoom="10"
        :center="{lat:14.564976132643977,lng:-90.46595274075851}"
        ref="map"
        >

        </GmapMap>


    </div>

</template>

<script>
export default {
    middleware: 'authenticated', 
}
</script>