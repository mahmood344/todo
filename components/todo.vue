<template>
  <v-card variant="outlined" class="mx-auto rounded" max-width="700">
    <v-card-title class="text-center">{{ sendData.dayName }}</v-card-title>
    <v-card-text>
        <v-form>
            <v-text-field :rules="[rules.required]" v-model="todoTitle" variant="outlined" density="compact" color="primary" label="وظیفه">
                <template v-slot:prepend-inner>
                    <v-btn :disabled="todoTitle.length <= 0" v-on:click="sendTitleJob(sendData.dayName)" size="small" color="success" variant="plain" icon="mdi-plus"></v-btn>
                </template>
            </v-text-field>
        </v-form>
        <v-list v-if="sendData.jobs.length > 0" class="text-center">
            <v-list-title>لیست کارها</v-list-title>
            <v-list-item variant="rounded-xl" class="text-right mt-3" v-for="(todo , index) in sendData.jobs" :key="index">
                <span :class="{active:todo.done}">{{ todo.text }}</span>
                <template v-slot:append>
                    <v-btn :disabled="todo.done" v-on:click="doneJob(sendData.dayName , todo)" size="small" color="success" variant="plain" icon="mdi-check-all"></v-btn>
                    <v-btn v-on:click="removeJob(sendData.dayName , todo)" size="small" color="error" variant="plain" icon="mdi-delete"></v-btn>
                </template>
                <v-divider></v-divider>
            </v-list-item>
        </v-list>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
    props:['sendData'],
    setup(props , context){
        const rules = {
        required: value => !!value || 'لطفا وظیفه خود را وارد نمایید',
      }
        const todoTitle = ref('');
        function sendTitleJob(dayName){
            context.emit('sendDataInput' , {
                title:todoTitle.value,
                name:dayName
            });
            todoTitle.value = '';
        }
        function doneJob(dayName , todo){
            console.log(dayName , todo);
            context.emit('sendDoneJob' , {
                name:dayName,
                text:todo.text
            });
        }
        function removeJob(dayName , todo){
            context.emit('sendRemoveDoneJob' , {
                name:dayName,
                text:todo.text
            });
        }
        return{todoTitle , sendTitleJob , doneJob , removeJob , rules}
    }
}
</script>

<style>
.active{
    text-decoration: line-through!important;
    color: green;
    opacity: 0.5;
}
</style>