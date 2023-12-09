<template>
  <v-app>
    <v-container>
      <v-row>
        <v-col cols="12" v-for="(data , index) in datas" :key="index">
          <todo :sendData="data" v-on:sendRemoveDoneJob="getRemoveDoneJob" v-on:sendDataInput="getDataInput" v-on:sendDoneJob="getDoneJob"></todo>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
export default {
  setup(){
    const datas = ref([{
      dayName:'شنبه',
      jobs:[]
    },
    {
      dayName:'یکشنبه',
      jobs:[]
    },
    {
      dayName:'دوشنبه',
      jobs:[]
    },
    {
      dayName:'سه شنبه',
      jobs:[]
    },
    {
      dayName:'چهارشنبه',
      jobs:[]
    },
    {
      dayName:'پنج شنبه',
      jobs:[]
    }]);
    function getDataInput(dataInput){
      console.log(dataInput);
      let result = datas.value.find((todo)=>{
        return todo.dayName == dataInput.name;
      })
      result.jobs.push({
        text:dataInput.title,
        done:false
      })
       console.log(result);
    };
    function getDoneJob(donJob){
      let resultFirst = datas.value.find((todo)=>{
        return todo.dayName == donJob.name;
      })
      let resultSeceond = resultFirst.jobs.find((todo)=>{
        return todo.text == donJob.text;
      })
      resultSeceond.done = true
    }
    function getRemoveDoneJob(donJob){
      let resultFirst = datas.value.find((todo)=>{
        return todo.dayName == donJob.name;
      })
       resultFirst.jobs = resultFirst.jobs.filter((todo)=>{
        return todo.text != donJob.text;
      })
    }
    return{datas , getDataInput , getDoneJob , getRemoveDoneJob}
  }
}
</script>

<style>
*{
  font-family: 'vazir';
  direction: rtl;
}
.border{
  border: 1px solid black;
}
</style>