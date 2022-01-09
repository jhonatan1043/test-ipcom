<template>
  <div class="container">
    <div class="card">
      <div class="card-header">
        <h5 class="title">Calendario</h5>
        <hr class="divider" />
        <div class="button-calendar">
          <button @click="makeCalendar(-5)" class="btn" type="button" v-text="'<'"></button>
          <p class="btn-divide">
            {{ diaInit + "/" + mes }} - {{ diaFin + "/" + mes }}
          </p>
          <button @click="makeCalendar(5)" class="btn" type="button" v-text="'>'"></button>
        </div>
      </div>
      <div class="card-body">
        <table>
          <thead>
             <th></th>
            <th v-for="(item, index) in calendarHeader" :key="index">
              {{ item.name }}
            </th>
          </thead>
          <tbody>
            <tr v-for="(item, index) in dataCalendar" :key="index">
                <td>{{item.hour}}</td>
                <td></td>
                <td></td>
                <td></td>
                <td></td>
                <td></td>
                <td></td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="card-footer">
           <label class="label">Seleccione:</label>
           <input type="date" class="control">
           <input type="time" class="control">
           <input type="button" class="btn-agenda" value="Agendar">
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";

export default {
  data() {
    return {
      dateCalendar: moment(),
      calendarHeader: [],
      url: "https://frontend.recruit.ipcom.ai",
      mes: 0,
      diaInit: 0,
      diaFin: 0,
      dataCalendar: [],
      hours: [],
    };
  },
  methods: {
    data(){
        this.dataCalendar = [ {hour : '8:00'},
                        {hour : '9:00'},
                        {hour : '10:00'},
                        {hour : '11:00'},
                        {hour : '12:00'},
                        {hour : '13:00'},
                        {hour : '14:00'},
                        {hour : '15:00'},
                        {hour : '16:00'},
                        {hour : '17:00'},
                        {hour : '18:00'},
                        {hour : '19:00'},
                        {hour : '20:00'},  ]
    },
    availableHours(date) {
      axios({
        url: `${this.url}/freeschedule`,
        params: {
          day: date,
        },
        method: "get",
        headers: { Authorization: "Bearer y8si7YgPII0t" },
      })
        .then((resp) => {
          this.hours = resp.data;
        })
        .catch(console.log);
    },
    makeCalendar(value) {
      let dateNow = this.dateCalendar.clone();
      let dateFin = dateNow.day(value);
      let dayInit = this.dateCalendar.format('DD');
      let dayFin = dateFin.format("DD");
     
      this.mes = dateNow.format("MM");
      this.diaInit = dayInit;
      this.diaFin = dayFin;

       this.calendarHeader = [];

      for (let i = 0; i <= 5; i++) {
        this.calendarHeader.push({ name: parseInt(dayInit) + i + "/" + this.mes });
      }

      this.data();

    },
  },
  mounted() {
    this.makeCalendar(5);
  },
};
</script>

<style scoped>
.container {
  display: flex;
  padding: 5px;
  justify-content: center;
}

.card {
  background-color: white;
  border: 1px solid #dfdad9;
  min-height: 550px;
  width: 90%;
  margin-top: 20px;
  height: 100%;
  border-radius: 12px;
}

.card-header {
  height: 60px;
  background-color: white;
}

.title {
  padding: 10px;
  padding-left: 13px;
  font-size: 23px;
  font-weight: 600;
}

.divider {
  border: 1px solid #dfdad9;
  margin-left: 14px;
  margin-right: 14px;
}

.button-calendar {
  display: flex;
  justify-content: center;
  font-size: 15px;
  padding-top: 12px;
  color: hsl(15, 2%, 55%);
}

.btn {
  background-color: transparent;
  color: hsl(15, 2%, 55%);
}

.btn-divide {
  margin-left: 15px;
  margin-right: 15px;
}

.card-body {
  height: 336px;
  position: relative;
  top: 40px;
  display: flex;
  justify-content: center;
}

table {
  border: 1px solid #dfdad9;
}
th,
td {
  width: 150px;
  text-align: center;
  vertical-align: top;
  border: 1px solid #dfdad9;
  font-size: 12px;
  font-weight: 200;
  border-collapse: collapse;
}
.card-footer{
  position: relative;
  bottom: -42px;
  display: flex;
  height: 80px;
  padding: 20px;
}

.control {
  border: 1px solid #3a2d2b;
  height: 20px;
  margin: 5px;
  border-radius: 5px;
  padding: 5px;
}

.label{
  position: relative;
  top: 12px;
}

.btn-agenda{
    background-color: #0083ff;
    height: 33px;
    margin: 5px;
    border-radius: 5px;
    padding: 9px;
    color: white;
    cursor: pointer;
}

</style>