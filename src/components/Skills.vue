<template>
  <div class="container">
    <div class="holder">
      <div class="invoice-box">
        <section>
                <div class="row" v-show="showForm" v-cloak>
                  <form @submit.prevent="addAppointment">
                    <div class="col-md-3">
                        <label class="sr-only" for="apptDate">Date</label>
                        <input type="date" v-model="newAppointment.apptDate" class="form-control" id="apptDate" >
                        {{ newAppointment.description }}
                    </div>
                    <div class="col-md-3">
                        <label  for="apptTime">Time</label>
                        <input type="time" class="form-control" id="apptTime" v-model="newAppointment.apptTime">
                    </div>
                    <div class=" col-md-4">
                        <label class="sr-only" for="description">Description</label>
                        <input type="text" class="form-control" v-model="newAppointment.description" name="description" v-validate="'min:5'" placeholder="Description">
                        <p class="alert" v-if="errors.has('description')"> {{ errors.first('description') }}</p>
                    </div>
                    <div class="col-md-2">
                        <button type="submit" class="btn btn-default">Add</button>
                    </div>
                  </form>
                </div>
                <hr>
            </section>
        <table class="table table-striped table-hover">
            <tr class="heading" v-show="appointments.length" v-cloak>
                <td>Date</td>
                <td>Time</td>
                <td colspan="2">Description</td>
            </tr>
            <tr class="item"  v-for="appointment in appointments" :key="appointment.id" v-show="appointments.length" v-cloak>
                <td>{{ appointment.apptDate }}</td>
                <td>{{ appointment.apptTime }}</td>
                <td colspan="2">{{ appointment.description }}</td>
            </tr>
        </table>
      </div>
      <div v-bind:class="alertObj"  v-show="showAlert" v-cloak> {{ validationMsg }} </div>
    </div>
  </div>
</template>

<script>
// localStorage persistence
var STORAGE_KEY = 'appointments-vuejs-2.0'
var appointmentsStorage = {
  fetch: function () {
    var appointments = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
    if (appointments.length === 0) {
       appointments = [ 
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
    appointments.forEach(function (appointment, index) {
      appointment.id = index;
    });
    appointmentsStorage.uid = appointments.length
    console.log(appointments);
    return appointments
  },
  save: function (appointments) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(appointments))
  }
}
export default {
  name: 'Skills',
  data() {
    return {
      name: 'Skills App',
      btnState: true,
      showForm: true,
      showAlert: false,
      validationMsg: '',
      appointments: appointmentsStorage.fetch(),
      newAppointment: {
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
    toggleForm: function() {
        if (this.showForm){
            this.showForm = false;
        } else
            {
                this.showForm = true; 
            }
    }, 
    addAppointment: function () {
      var value = this.newAppointment;
      this.newAppointment.id = appointmentsStorage.uid++;
      if (!value.apptTime) {
        this.validationMsg = 'Please enter a time with AM/PM included';
        this.showAlert = true;
        return
      }
      this.appointments.push(this.newAppointment);
      this.newAppointment = {
                description: '',
                apptDate: '',
                apptTime: ''
        }
    },

    removeAppt: function (appointment) {
      this.appointments.splice(this.appointments.indexOf(appointment), 1)
    },

    editAppointment: function (appointment) {
      this.beforeEditCache = appointment.description
      this.editedAppointment= appointment
    },

    doneEdit: function (appointment) {
      if (!this.editedAppointment) {
        return
      }
      this.editedAppointment = null
      appointment.description = appointment.description.trim()
      if (!appointment.description) {
        this.removeAppointment(appointment)
      }
    },

    cancelEdit: function (appointment) {
      this.editedAppointment = null
      appointment.description = this.beforeEditCache
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./Skills.css" scoped>
input {
    width: calc(100% - 40px);
    border: 0;
    padding: 20px;
    font-size: 1.3em;
    background-color: #323333;
    color: #687F7F;
  }
</style>

