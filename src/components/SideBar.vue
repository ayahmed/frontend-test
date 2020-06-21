<template>
    <div>
      <b-button v-b-toggle.sidebar-1><b-icon-list></b-icon-list></b-button>
        <b-sidebar id="sidebar-1" :visible="true"  shadow>
            <div>
                <b-form v-if="show">
                    <b-form-group id="input-group-3" label="Project Ownership:" label-for="input-3">
                        <b-form-select
                            id="input-3"
                            v-model="form.ownership"
                            :options="ownerships"
                            required
                            @change="onChangeOwnerShip(form.ownership)">
                        </b-form-select>
                    </b-form-group>
                    <b-form-group id="input-group-4" label="Project Status:" label-for="input-4">
                        <b-form-select
                            id="input-4"
                            v-model="form.status"
                            :options="statuses"
                            required
                            @change="onChangeStatus(form.status)">
                        </b-form-select>
                    </b-form-group>
                </b-form>
            </div>
            <div>
            </div>
            <b-list-group>
                <b-list-group-item v-for="feature of filteredData" :key="feature.id"> Project Title:
                    {{feature.properties.project.Title}}
                </b-list-group-item>
            </b-list-group>
        </b-sidebar>
    </div>
</template>
<script>
  import axios from 'axios';

    export default {
        name: 'SideBar',
        data() {
            return {
                data: [],
                filteredData: [],
                mapGeoJson: [],
                form: {
                    ownership: null,
                    status: null
                },
                ownerships: [],
                statuses: [],
                show: true
            }
        },
        methods: {
            onChangeOwnerShip(own) {
                this.filteredData = [];
                if (this.form.status == null) {
                    for (var i = 0; i < this.mapGeoJson.features.length; i++) {
                        if (this.mapGeoJson.features[i].properties.project.Ownership == own) {
                            this.filteredData.push(this.mapGeoJson.features[i])
                        }
                    }
                } else {
                    for (var j = 0; j < this.mapGeoJson.features.length; j++) {
                        if (this.mapGeoJson.features[j].properties.project.Status == this.form.status && this.mapGeoJson.features[j].properties.project.Ownership == own) {
                            this.filteredData.push(this.mapGeoJson.features[j])
                        }
                    }
                }
                this.emitParent();
            },
            onChangeStatus(status) {
                this.filteredData = [];
                if (this.form.ownership == null) {
                    for (var i = 0; i < this.mapGeoJson.features.length; i++) {
                        if (this.mapGeoJson.features[i].properties.project.Status == status) {
                            this.filteredData.push(this.mapGeoJson.features[i])
                        }
                    }
                } else {
                    for (var j = 0; j < this.mapGeoJson.features.length; j++) {
                        if (this.mapGeoJson.features[j].properties.project.Status == status && this.mapGeoJson.features[j].properties.project.Ownership == this.form.ownership) {
                            this.filteredData.push(this.mapGeoJson.features[j])
                        }
                    }
                }
                this.emitParent();
            },
            loadJson() {
                return axios.get('https://bitbucket.org/idda/coding-challenges/raw/88be221f75a1b108c9e5f7222906b2735c147ac8/testBlob.json');
            },
            setDataFilters() {
                this.loadJson().then(res => {
                    const featureData = res.data.features;
                    this.mapGeoJson = res.data;
                    this.filteredData = featureData;
                    for (var i = 0; i < featureData.length; i++) {
                        if (!this.ownerships.includes(featureData[i].properties.project.Ownership)) {
                            this.ownerships.push(featureData[i].properties.project.Ownership);
                        }
                        if (!this.statuses.includes(featureData[i].properties.project.Status)) {
                            this.statuses.push(featureData[i].properties.project.Status);
                        }
                    }
                    this.emitParent();
                });
            },
            emitParent(){
              this.$parent.filterUpdate(this.filteredData);
            }
        },
        created() {
            this.setDataFilters();
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
button.btn.btn-secondary.collapsed {
    position: absolute;
    z-index: 9;
    left: 10px;
    top: 10px;
}
</style>
