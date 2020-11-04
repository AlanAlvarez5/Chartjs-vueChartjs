<template>
  <v-container>
    <v-row cols="12" justify="center" align="start">

      <v-col lg="4" md="6" sm="8" cols="12" justify="center" align="center">
        <v-card  >
          <v-card-title>Consumo Mensual</v-card-title>
          <v-card-text>
            <line-chart :chart-data="monthData" :options="monthOptions"></line-chart>
            <v-row>
              <v-col lg="4" md="6" sm="8" cols="12">
                <v-select :items="meses" item-text="mes" item-value="val" label="Mes" v-model="mes"></v-select>
              </v-col>
              <v-col lg="4" md="6" sm="8" cols="12">
                <v-select :items="anios" label="Año" v-model="anio"></v-select>
              </v-col>
              <v-col lg="4" md="6" sm="8" cols="12">
                <v-switch
                  class="ml-5"
                  v-model="switch4"
                  inset
                  label="Mostrar low"
                ></v-switch>
              </v-col>
            </v-row>
            <v-row justify="center" align="center">
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>

      <v-col lg="4" md="6" sm="8" cols="12" >
        <v-card>
          <v-card-title>Consumo de hoy</v-card-title>
          <v-card-text>
            <line-chart :chart-data="dayData" :options="dayOptions"></line-chart>
            <v-row v-col lg="4" md="6" sm="8" cols="12" align="center" justify="center">
              <v-btn dark @click="fillDayData"> Randomize</v-btn>
              <v-switch
                class="ml-5"
                v-model="switch2"
                inset
                label="Mostrar 2"
              ></v-switch>
              <v-switch
                class="ml-5"
                v-model="switch3"
                inset
                label="Mostrar 3"
              ></v-switch>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>

    </v-row>
  </v-container>
</template>

<script>
// @ is an alias to /src
import LineChart from '../charts/LineChart'

export default {
  name: 'Home',
  components: {
    LineChart,
  },
  mounted() {

    this.file = require('../AAPL.json')
    this.file2 = require('../data_hour.json')

    this.fillmonthData()
    this.fillDayData()
  },
  data() {
    return {
      file: null,
      file2: null,

      chartStyle: {
        height: '300px'
      },

      monthData: null,
      monthOptions: null,
      switch4: false,

      dayData: null,
      dayOptions: null,
      switch2: false,
      switch3: false,

      meses: [
        {mes: 'Enero', val: 0},
        {mes: 'Febrero', val: 1},
        {mes: 'Marzo', val: 2},
        {mes: 'Abril', val: 3},
        {mes: 'Mayo', val: 4},
        {mes: 'Junio', val: 5},
        {mes: 'Julio', val: 6},
        {mes: 'Agosto', val: 7},
        {mes: 'Septiembre', val: 8},
        {mes: 'Octubre', val: 9},
        {mes: 'Noviembre', val: 10},
        {mes: 'Diciembre', val: 11},
      ],
      mes: new Date().getMonth(),

      anios: [2014, 2015, 2016, 2017, 2018, 2019],
      anio: 2014,

    }
  },
  watch: {
    mes: function(){
      this.fillmonthData()
    },
    anio: function(){
      this.fillmonthData()
    },
    switch2: function(){
      this.fillDayData()
    },
    switch3: function(){
      this.fillDayData()
    },
    switch4: function(){
      this.fillmonthData()
    },
    
  },
  methods: {
  
    fillmonthData(){

      let high = []
      let low = []

      this.file.forEach(
        data => {
          let newDate = new Date(data.Date)
          if(newDate.getMonth() == this.mes && newDate.getFullYear() == this.anio){
            high.push({
              x: data.Date,
              y: data.High
            })
            low.push({
              x: data.Date,
              y: data.Low
            })
          }
        }
      )

      this.monthData = {
        datasets: [
          {
            label: 'High',
            data: high,
            borderColor: '#FF0000',
            backgroundColor: '#FF000000',
            type: 'line'
          },
          {
            label: 'Low',
            data: this.switch4? low: null,
            borderColor: '#00FF00',
            backgroundColor: '#00FF0000',
            type: 'line'
          },
        ]
      }

      this.monthOptions = {
        scales: {
          xAxes: [
            {
              type: 'time',
              time: {
                // unit: this.unit
                source: 'auto'
              },
              scaleLabel: {
                display: false,
                labelString: 'Fechas'
              }
            },
          ],
          yAxes: [
            {
              ticks: {
                callback: function(value, index, values){
                  return value + ' units'
                }
              },
              scaleLabel: {
                display: true,
                labelString: 'Units'
              }
            },
          ]
        },
        title: {
            display: false,
            text: 'Custom Chart Title'
        },
        legend: {
          position: 'top',
          reverse: false
        }
      }
    },

    fillDayData(){
      let vals = []
      let vals2 = []
      let vals3 = []
      var i = 0
      this.file2.forEach( val => {

        if (i % 4 == 0){
          vals.push(
            {
              x: val.date,
              y: this.getRandomInt()
            }
          )
          vals2.push(
            {
              x: val.date,
              y: this.getRandomInt()
            }
          )
          vals3.push(
            {
              x: val.date,
              y: this.getRandomInt()
            }
          )
        }

        i += 1
      })

      this.dayData = {
        datasets: [
          {
            label: 'High',
            data: vals,
            borderColor: 'cyan',
            backgroundColor: 'cyan',
            type: 'bar'
          },
          {
            label: '2',
            data: this.switch2? vals2: null,
            borderColor: 'green',
            backgroundColor: 'green',
            type: 'bar'
          },
          {
            label: '3',
            data: this.switch3? vals3: null,
            borderColor: 'orange',
            backgroundColor: 'orange',
            type: 'bar'
          },
        ]
      }

      this.dayOptions = {
        scales: {
          xAxes: [
            {
              type: 'time',
              time: {
                // unit: this.unit
                source: 'auto'
              },
              scaleLabel: {
                display: false,
                labelString: 'Fechas'
              }
            },
          ],
          yAxes: [
            {
              ticks: {
                callback: function(value, index, values){
                  return value + ' units'
                }
              },
              scaleLabel: {
                display: true,
                labelString: 'Units'
              }
            },
          ]
        },
        title: {
            display: false,
            text: 'Custom Chart Title'
        },
        legend: {
          position: 'top',
          reverse: false
        }
      }

    },

    getRandomInt() {
      return Math.floor(Math.random() * (90 - 20)) + 20
    }
  },
}
</script>
