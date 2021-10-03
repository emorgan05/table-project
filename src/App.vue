<template>
  <div id="app">
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
      </div>
      <table>
        <thead>
          <tr>
            <th></th>
            <th v-for="col in columns" :key="col.props">{{ col.label }}</th>
            <th>Status</th>
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
            <td v-for="col in columns" :key="col.props">{{ row[col.props] }}</td>
            <td>
              <span v-if="row.status === 'available'" class="dot"></span> {{ row.status.charAt(0).toUpperCase() + row.status.slice(1) }}
            </td>
          </tr>
        </tbody>
      </table>
    </section>
  </div>
</template>

<script>
export default {
  name: 'app',
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
        }
      ],
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

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

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
  margin-left: 14px;
  margin-right: 14px;
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
