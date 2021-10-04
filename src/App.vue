<template>
    <div id="app">
        <table-component
            :table-data="tableData"
            :columns="columns">
        </table-component>
    </div>
</template>

<script>
import TableComponent from './table.vue';

export default {
    name: 'app',
    components: {
        TableComponent
    },
    data () {
        return {
            tableData: [
                {name: 'smss.exe', device: 'Stark', path: '\\Device\\HarddiskVolume2\\Windows\\System32\\smss.exe', status: 'scheduled'},
                {name: 'netsh.exe', device: 'Targaryen', path: '\\Device\\HarddiskVolume2\\Windows\\System32\\netsh.exe', status: 'available'},
                {name: 'uxtheme.dll', device: 'Lannister', path: '\\Device\\HarddiskVolume1\\Windows\\System32\\uxtheme.dll', status: 'available'},
                {name: 'cryptbase.dll', device: 'Martell', path: '\\Device\\HarddiskVolume1\\Windows\\System32\\cryptbase.dll', status: 'scheduled'},
                {name: '7za.exe', device: 'Baratheon', path: '\\Device\\HarddiskVolume1\\temp\\7za.exe', status: 'scheduled'}
            ],
            columns: [
                {
                    props: 'name',
                    label: 'Name'
                },
                {
                    props: 'device',
                    label: 'Device'
                },
                {
                    props: 'path',
                    label: 'Path'
                },
                {
                    props: 'status',
                    label: 'Status'
                }
            ]
        }
    }
}
</script>

<style>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
