<template>
    <section class="table">
        <div class="table-title">
            <input
                class="select-all-checkbox"
                type="checkbox"
                id="selectAll"
                name="selectAll"
                v-model="checked"
                @change="handleSelectAll" />
            <h4 class="selected-text">{{ selectedRowsTitle }}</h4>
            <button class="download-button" @click="downloadSelectedRows">
                <img src="./assets/download.svg" width="16" height="16" />
                Download Selected
            </button>
            <slot name="table-header" />
        </div>
      <table>
        <thead>
            <tr>
                <th></th>
                <th v-for="col in columns" :key="col.props">{{ col.label }}</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="row in tableData" :key="row.name" :class="{ selected: row.checked }">
                <td>
                    <input
                        type="checkbox"
                        :id="row.name"
                        :checked="row.checked"
                        @change="handleSelectRow(row)" />
                </td>
                <td v-for="col in columns" :key="col.props">
                    <template v-if="col.props !== 'status'">
                        {{ row[col.props] }}
                    </template>
                    <template v-else>
                        <span v-if="row.status === 'available'" class="dot"></span> {{ row.status.charAt(0).toUpperCase() + row.status.slice(1) }}
                    </template>
                </td>
            </tr>
        </tbody>
      </table>
    </section>
</template>

<script>
export default {
    name: 'TableComponent',
    props: {
        tableData: {
            type: Array,
            default: () => []
        },
        columns: {
            type: Array,
            default: () => []
        }
    },
    data () {
        return {
            checked: false,
            selectedRows: []
        }
    },
    computed: {
        selectedRowsTitle () {
            if (this.selectedRows.length) {
                return `Selected ${this.selectedRows.length}`;
            }

            return 'None Selected';
        }
    },
    methods: {
        handleSelectAll () {
            let selectAll = document.querySelector('input[id="selectAll"]');

            if (this.tableData.length === this.selectedRows.length) {
                this.selectedRows = [];
                this.checked = false;
                this.tableData.forEach(row => {
                    row.checked = false;
                });
            } else {
                this.selectedRows = this.tableData;
                this.checked = true;
                this.tableData.forEach(row => {
                    row.checked = true;
                });
            }

            selectAll.indeterminate = false;
        },
        handleSelectRow (row) {
            const selectedIndex = this.selectedRows.findIndex(selectedRow => selectedRow.name === row.name);
            let selectAll = document.querySelector('input[id="selectAll"]');

            if (selectedIndex >= 0) {
                this.selectedRows.splice(selectedIndex, 1);
                row.checked = false;
            } else {
                this.selectedRows.push(row);
                row.checked = true;
            }

            if (this.selectedRows.length === 0) {
                this.checked = false;
                selectAll.indeterminate = false;
            } else if (this.selectedRows.length !== this.tableData.length) {
                this.checked = false;
                selectAll.indeterminate = true;
            } else if (this.selectedRows.length === this.tableData.length) {
                this.checked = true;
                selectAll.indeterminate = false;
            }
        },
        downloadSelectedRows () {
            const alertText = this.selectedRows.map(row => {
                if (row.status === 'available') {
                    return `${row.path} -- ${row.device}`;
                }
            }).join(', ');

            alert(alertText);
        }
    }
}
</script>

<style scoped>
.table {
    border: 1px solid #D3D3D3;
    box-shadow: 5px 5px 10px #D3D3D3;
}

.table-title {
    display: flex;
    flex-direction: row;
    margin: 10px 0px;
}

.select-all-checkbox {
    margin-left: 18px;
    margin-right: 18px;
}

.selected-text {
    margin: 0;
    padding-left: 18px;
    padding-right: 24px;
}

.download-button {
    display: inline-block;
    border: none;
    margin: 0;
    text-decoration: none;
    background: white;
    color: black;
    font-size: 1rem;
    line-height: 1;
    cursor: pointer;
    text-align: center;
    -webkit-appearance: none;
    -moz-appearance: none;
}

.download-button:hover,
.download-button:focus {
    background: #E0E0E0;
}

table {
    width: 100%;
    border-collapse: collapse;
    border: 1px solid #D3D3D3;
    padding: 10px;
}

tr {
    border: 1px solid #D3D3D3;
}

tr:hover,
tr:focus {
    background-color:	#F5F5F5;
}

.selected {
    background-color: azure;
}

th, td {
    height: 40px;
}

.dot {
    height: 15px;
    width: 15px;
    background-color: #50C878;
    border-radius: 50%;
    display: inline-block;
}
</style>