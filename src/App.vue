<template>
    <div id="app">
        <SideBar />
        <MapContainer ref="map" />
    </div>
</template>

<script>
    import SideBar from './components/SideBar.vue'
    import MapContainer from './components/MapContainer.vue'

    import 'bootstrap/dist/css/bootstrap.css'
    import 'bootstrap-vue/dist/bootstrap-vue.css'

    export default {
        name: 'App',
        components: {
            SideBar,
            MapContainer
        },
        methods: {
            filterUpdate(data) {
                this.$refs.map.updateMapData(data);
            }
        },
        data() {
            return {
                filteredData: []
            }
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    .mapboxgl-marker {
        cursor: pointer;
    }
</style>
