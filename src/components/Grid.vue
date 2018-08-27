<template>
  <div class="container">
    <div class="holder">
      <div class="invoice-box" id="example">
        
      </div>
    </div>
  </div>
</template>

<script>

// localStorage persistence
var STORAGE_KEY = 'appointments-vuejs-2.0'
var gridStorage = {
  fetch: function () {
    var gridData = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
    if (gridData.length === 0) {
       gridData = [ 
            {
                description: 'Appointment to get hair done.',
                apptDate: 'July 3 2018',
                apptTime: '12:00'
            },
            {
                description: 'Dr Stover appointment',
                apptDate: 'July 11 2018',
                apptTime: '14:30'
            },
            {
                description: 'Another appointment',
                apptDate: 'July 21 2018',
                apptTime: '09:00'
            }]
    }  
    gridData.forEach(function (gridRow, index) {
      gridRow.id = index;
    });
    gridStorage.uid = gridData.length
    console.log(gridData);
    return gridData
  },
  save: function (gridData) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(gridData))
  }
}

export default {
  name: 'GridComponent',
  data() {
    return {
      name: 'Grid Component',
      gridData: gridStorage.fetch(),
      newGridRow: {
                id: '',
                description: '',
                apptDate: '',
                apptTime: ''
        },
      alertObj: {
        alert: true, 
        alertText: true 
        }
    }
  },
   methods: {
    addRow: function () {
      var value = this.newGridRow;
      this.newGridRow.id = gridStorage.uid++;
      if (!value.apptTime) {
        this.validationMsg = 'Please enter a time with AM/PM included';
        this.showAlert = true;
        return
      }
      this.gridData.push(this.newGridRow);
      this.newGridRow = {
                description: '',
                apptDate: '',
                apptTime: ''
        }
    },

    removeGridRow: function (gridRow) {
      this.gridData.splice(this.gridData.indexOf(gridRow), 1)
    },

    editGridRow: function (gridRow) {
      this.beforeEditCache = appointment.description
      this.editedAppointment= appointment
    },

    doneEdit: function (gridRow) {
      if (!this.editedGridRow) {
        return
      }
      this.editedGridRow = null
      gridRow.description = gridRow.description.trim()
      if (!gridRow.description) {
        this.removeGridRow(gridRow)
      }
    },

    cancelEdit: function (gridRow) {
      this.editedGridRow = null
      gridRow.description = this.beforeEditCache
    },

    removeCompleted: function () {
      this.gridData = filters.active(this.gridData)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./Skills.css" scoped></style>

