<script setup lang="ts">
import { ref } from 'vue'
import SelectBox from './SelectBox.vue'
import nameForm from './nameForm.vue'

const month1 = ref(0);
const date1   = ref(0);
const name1  = ref("");

const month2 = ref(0);
const date2   = ref(0);
const name2  = ref("");

const hide   = ref(true);
const exist  = ref(true);

const midMonth = ref(0);
const midDate   = ref(0);

const thisYear = () => new Date().getFullYear();

function checkExist(month: number, day: number): boolean {
    const date = new Date(thisYear(), month, day);
    if(date.getMonth() === month && date.getDay() === day){
        return true;
    }
    return false;
}

function clcuMidBirthDay() {
    hide.value = false;
    const e1 = checkExist(month1.value, date1.value);
    const e2 = checkExist(month2.value, date2.value);
    exist.value = (!e1) || (!e2);

    //初期値ならエラー
    if(name1.value === "" || month1.value === 0 || date1.value === 0) exist.value = false;
    if(name1.value === "" || month2.value === 0 || date2.value === 0) exist.value = false;

    if(exist){
        const d1 = Date.parse(new Date(thisYear(), month1.value-1, date1.value).toString());
        const d2 = Date.parse(new Date(thisYear(), month2.value-1, date2.value).toString());
        const midBirth  = new Date((d1+d2)/2);
        midMonth.value  = midBirth.getMonth() + 1;
        midDate.value   = midBirth.getDate() + (midBirth.getHours() === 12 ? 1 : 0);
    }
}

function twitterShare(){
    const shareURL = 'https://twitter.com/intent/tweet?text=%23推しカプ真ん中バースデー%0a' + `${name1.value} と ${name2.value} の真ん中バースデーは \n ${midMonth.value}月${midDate.value}日！`; 
    window.open(shareURL, "_blank");
}

const setMonth1 = (month: number) => month1.value = month;
const setDate1   = (day: number)  => date1.value   = day;
const setName1  = (name: string)  => name1.value  = name;

const setMonth2 = (month: number) => month2.value = month;
const setDate2   = (day: number)  => date2.value   = day;
const setName2  = (name: string)  => name2.value  = name;

</script>

<template>
  <div class="text-base sm:text-lg md:text-xl lg:text-2xl xl:text-3xl">
    <div class="pb-8 text-center font-bold text-3xl sm:text-3xl md:text-4xl lg:text-5xl xl:text-6xl">
        <h1> 推しカプ <br> 真ん中バースデー！</h1>
    </div>
    
    <div class="space-y-4">
      <div class="flex flex-row justify-center space-x-4 h-8 sm:h-16 md:h-20 lg:h-24 xl:h-28">
        <nameForm  class="rounded-md border-2" v-on:input ="setName1"   :placeholder="'おなまえ１'"></nameForm>
        <SelectBox v-on:select="setMonth1"  :placeholder="'月'" :min=1 :max=12></SelectBox>
        <SelectBox v-on:select="setDate1"   :placeholder="'日'" :min=1 :max=31></SelectBox>
      </div>
      <div class="flex flex-row justify-center space-x-4 h-8 sm:h-16 md:h-20 lg:h-24 xl:h-28">
        <nameForm  class="rounded-md border-2" v-on:input ="setName2"   :placeholder="'おなまえ２'"></nameForm>
        <SelectBox v-on:select="setMonth2"  :placeholder="'月'" :min=1 :max=12></SelectBox>
        <SelectBox v-on:select="setDate2"   :placeholder="'日'" :min=1 :max=31></SelectBox>
      </div>
      <button class="shadow-md button border-2 font-bold bg-gray-600 text-white rounded-full h-16 sm:h-16 md:h-20 lg:h-24 xl:h-28 w-16 sm:w-16 md:w-20 lg:w-24 xl:w-28 justify-center" @click=clcuMidBirthDay>計算</button>
    </div>
    <div v-if="!hide" class="pb-8 pt-8">
      <div v-if="exist">
          <p class="text-gray-700 text-center font-bold text-3xl sm:text-3xl md:text-4xl lg:text-5xl xl:text-6xl"> {{ name1 }} と {{ name2 }} <br> の真ん中バースデーは </p>
          <p class="text-gray-700 text-center pb-8 pt-8 font-bold text-4xl sm:text-4xl md:text-5xl lg:text-6xl xl:text-7xl"> {{ midMonth }}月  {{ midDate }}日！</p>
          <button @click="twitterShare" class="shadow-md bg-blue-400 text-white rounded-full py-2 px-4">Twitterでシェアする</button>
      </div>
      <h1 v-else>入力されていない項目があります</h1>
    </div>
   </div>
</template>

<style scoped>
input {
    background-color: #cfcfcf;
}
</style>
