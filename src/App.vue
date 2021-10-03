<template>
  <div id="app">
    <section class="table">
      <div class="table-title">
        <input type="checkbox" />
        <h4 class="selected-text">{{ selectedRowsTitle }}</h4>
        <button class="download-button" @click="downloadSelectedRows">
          <img src="./assets/download.svg" width="16" height="16" />
          Download Selected
        </button>
      </div>
      <table>
        <!-- selectAll checkbox -- unselected, selected, indeterminate
            selects all if none or some selected. clear if all are currently selected -->
        <thead>
          <tr>
            <th v-for="col in columns" :key="col.props">{{ col.label }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="row in tableData" :key="row.name">
            <td v-for="col in columns" :key="col.props">{{ row[col.props] }}</td>
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
        // checkbox column
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
          // add an icon
          // format string to be capitalized
        }
      ],
      selectedRows: [
        {name: 'netsh.exe', device: 'Targaryen', path: '\\Device\\HarddiskVolume2\\Windows\\System32\\netsh.exe', status: 'available'},
        {name: 'uxtheme.dll', device: 'Lannister', path: '\\Device\\HarddiskVolume1\\Windows\\System32\\uxtheme.dll', status: 'available'}
      ]
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
    // selectable rows
      // change color on select and on hover
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

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
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

.selected-text {
  margin: 0;
  padding: 0 12px;
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

button:hover,
button:focus {
    background: #E0E0E0;
}

button:focus {
    outline: 1px solid #E0E0E0;
    outline-offset: -4px;
}

button:active {
    transform: scale(0.99);
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

th, td {
  height: 40px;
}
</style>
