
<script setup>
    import DataTable from 'datatables.net-vue3';
    import DataTablesCore from 'datatables.net';
    import 'datatables.net-buttons';
    import 'datatables.net-buttons/js/buttons.html5';
    import 'datatables.net-responsive';
    import 'datatables.net-select';
    import mapboxgl from "mapbox-gl";

    DataTable.use(DataTablesCore);

    const columns=[
        { data: 'commit.author.name' },
        { data: 'commit.author.date' },
        { data: 'commit.message' }
    ]

    const options = {
        // select: true,
        // responsive: true,
        ajax: {
            url: 'https://api.github.com/repos/DataIntegrationGroup/NMBGFastAPI/commits',
            dataSrc: ''
        },

    }
</script>

<script>
export default {
    data() {
        return {
            loading: false,
            location: "",
            // access_token: process.env.VUE_APP_MAP_ACCESS_TOKEN,
            center: [-106.5, 34.5],
            map: {},
        };
    },

    mounted() {
        this.createMap();
    },

    methods: {
        async createMap() {
            try {
                mapboxgl.accessToken = "pk.eyJ1IjoiamFrZXJvc3N3ZGkiLCJhIjoiY2s3M3ZneGl4MGhkMDNrcjlocmNuNWg4bCJ9.4r1DRDQ_ja0fV2nnmlVT0A";
                this.map = new mapboxgl.Map({
                    container: "map",
                    style: "mapbox://styles/mapbox/streets-v11",
                    center: this.center,
                    zoom: 7,
                });
                const nav = new mapboxgl.NavigationControl();
                this.map.addControl(nav, "top-right");

            }catch (e) {
                console.log(e)
            }
        }
    }
}

</script>

<template>
    <header>

    </header>
    <div class="main">
        <div>
            <h1>CollabNet</h1>
            <hr>
            <div class="map-holder">
                <div id="map"></div>
            </div>
            <hr>
            <DataTable
                    :columns="columns"
                    :options="options"
                    class="display nowrap"
                    width="100%"
            >
                <thead>
                <tr>
                    <th>Name</th>
                    <th>Position</th>
                    <th>Office</th>

                </tr>
                </thead>
                <tfoot>
                <tr>
                    <th>Name</th>
                    <th>Position</th>
                    <th>Office</th>

                </tr>
                </tfoot>
            </DataTable>
        </div>
    </div>
</template>

<style>
@import 'datatables.net-dt';
@import 'datatables.net-buttons-dt';
@import 'datatables.net-responsive-dt';
@import 'datatables.net-select-dt';


.main {
    padding: 45px 50px;
}

.map-holder {
    width: 100%;
}
#map {
    height: 70vh;
}
</style>
