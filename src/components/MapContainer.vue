<template>
    <div class="map-container">
        <MglMap ref="myMap" :mapStyle="mapStyle" :accessToken="accessToken" @load="onMapLoad">
            <MglMarker :key="index" v-for="(data, index) in  markerData" :coordinates.sync="data.coordinates"
                       color="blue">
                <MglPopup>
                    <b-card class="mb-4 text-left no-border no-padding">
                        <b-card-title>{{data.projectDetail.Title}}</b-card-title>
                        <b-card-sub-title>{{data.projectDetail.Category}}</b-card-sub-title>
                        <b-card-sub-title>{{data.projectDetail.SubCategory}}</b-card-sub-title>
                        <b-card-text>
                            <ul>
                                <li><b>Stage</b>: {{data.projectDetail.Value}}</li>
                                <li><b>Type</b>: {{data.projectDetail.Type}}</li>
                                <li><b>Value</b>: {{data.projectDetail.Value}}</li>
                                <li><b>Ownership</b>: {{data.projectDetail.Ownership}}</li>
                                <li><b>Notes</b>: {{data.projectDetail.Notes}}</li>
                                <li><b>Suburb</b>: {{data.projectDetail.Suburb}}</li>
                                <li><b>State</b>: {{data.projectDetail.State}}</li>
                                <li><b>Council</b>: {{data.projectDetail.Council}}</li>
                                <li><b>Address</b>: {{data.projectDetail.Address}}</li>
                            </ul>
                        </b-card-text>
                    </b-card>
                </MglPopup>
            </MglMarker>
        </MglMap>
    </div>
</template>

<script>
    import {MglMap, MglMarker, MglPopup} from "vue-mapbox";

    export default {
        name: 'MapContainer',
        components: {
            MglMap,
            MglMarker,
            MglPopup
        },
        methods: {

            onMapLoad(event) {
                this.map = event.map;
                this.updateMarkerData();
            },
            updateMapData(geoJson) {
                this.mapGeoJson = geoJson;
                this.updateMarkerData();
            },
            updateMarkerData() {
                // generating markers for site map
                this.markerData = [];
                const mapJson = this.mapGeoJson;
                for (let i = 0; i < mapJson.length; i++) {
                    let coordinates = mapJson[i].geometry.coordinates;
                    let projectDetail = mapJson[i].properties.project;
                    this.markerData.push({
                        coordinates: coordinates,
                        projectDetail: projectDetail
                    });
                }

                if (this.map !== null && typeof this.markerData[0] !== "undefined"){
                    const coordinates = this.markerData[0].coordinates;
                    this.map.flyTo({
                        center: coordinates,
                        zoom: 15,
                        speed: 1
                    });
                }
            }
        },
        data() {
            return {
                accessToken: 'pk.eyJ1IjoiYXlhaG1lZCIsImEiOiJja2JtYmRrajExaDB4MnJsOXkwZjQ1Zzg3In0.1OLvBIjAekwtAQQFAoqEiA',
                mapStyle: 'mapbox://styles/mapbox/streets-v11',
                mapGeoJson: [],
                markerData: [],
                map: null
            };
        }

    }
</script>

<style scoped>
    .map-container {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: 1;
    }

    .card {
        padding: 0;
        border: 0;
        margin: 13px 0 0 0 !important;
    }

    .card .card-body {
        padding: 0;
    }

    .card .card-body .card-title {
        font-size: 13px;
        font-weight: bold;
    }

    .card .card-body .card-subtitle {
        font-size: 12px;
        margin-bottom: 10px;
    }

    .card .card-body .card-text ul {
        padding: 0;
        margin: 0;
        list-style: none;
    }

    .card .card-body .card-text ul li {
        font-size: 11px;
    }

    .card .card-body .card-text ul li b {
        text-decoration: underline;
    }
</style>
