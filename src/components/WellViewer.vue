<script setup>
import { ref, onMounted } from 'vue';

import DataTablesLib from 'datatables.net';
import 'datatables.net-select';

import DataTable from 'datatables.net-vue3';
import DataTablesCore from 'datatables.net';
import 'datatables.net-buttons';
import 'datatables.net-buttons/js/buttons.html5';
import 'datatables.net-responsive';
import 'datatables.net-select';
import mapboxgl from "mapbox-gl";

// DataTable.use(DataTablesCore);
DataTable.use(DataTablesLib);
const data = ref([])
const table = ref()

const columns=[
    { data: 'commit.author.name' },
    { data: 'commit.author.date' },
    { data: 'commit.message' }
]

const options = {
    // select: true,
    // responsive: true,
    // ajax: {
    //     url: 'https://api.github.com/repos/DataIntegrationGroup/NMBGFastAPI/commits',
    //     dataSrc: ''
    // },

}

let dt;
let tinput;

onMounted(function () {
    dt = table.value.dt;
    tinput = tinput.value;
});

function searchDB() {
    // console.log(dt)
    // data.value.push({"commit": {"author": {"name": "test", "date": "test"}, "message": "test"}})
    // console.log(this.text)
    fetch('https://api.github.com/repos/DataIntegrationGroup/NMBGFastAPI/commits')
        .then(response => response.json())
        .then(dj => {
            console.log(dj)
            data.value = dj
        })
    console.log(dt.rows())
    // `this` points to the vm instance
    // console.log('asfaf', dt)
    // console.log('fasdfafafasfasd',dt)
}

</script>
<script>
export default {
    data(){
        return {
            text: ""
        }
    },
    methods: {
        handleInput(e) {
            this.text = e.target.value;
        }
    }
}
</script>
<template>
    <button @click="searchDB">Do Search</button>
    <input
            type="text"
            v-model="text"
            placeholder="Text"
            @input="handleInput"
    />
    <p>{{ text }}</p>

<DataTable
        :columns="columns"
        :options="options"
        :data="data"
        ref="table"
        class="display nowrap"
        width="100%"
>
</DataTable>
</template>

<style scoped>

</style>