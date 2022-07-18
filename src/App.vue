<template>
  <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)"></main-screen>
  <interact-screen v-if="statusMatch === 'match'" :cardContext="settings.cardContext" @onFinish="onGetResult"></interact-screen>
  <result-screen v-if="statusMatch === 'result'" :timer="timer" @onStartAgain="statusMatch = 'default'"></result-screen>
  <copy-right></copy-right>
</template>

<script>
import MainScreen from './components/MainScreen.vue'
import InteractScreen from './components/InteractScreen.vue'
import ResultScreen from './components/ResultScreen.vue'
import CopyRight from './components/CopyRight.vue'

import { shuffled } from "./utils/array.js";

export default {
  name: 'App',
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight,
  },
  data() {
    return {
      statusMatch: 'default',
      settings: {
        totalOfBlocks: 0,
        cardContext: [],
      },
      timer: 0,
    }
  },
  
  methods: {
    onHandleBeforeStart(config) {
      console.log('running handle before start...',config);
      this.settings.totalOfBlocks = config.totalOfBlocks;
      
      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2}, 
        (_, i) => i + 1 );

      const secondCards = [...firstCards]; // dùng cú pháp ES6 để copy mảng, gọi là clone shadow
      const cards = [...firstCards,...secondCards]; // dùng cú pháp ES6 để mix 2 mảng lại

      console.log(cards);
      this.settings.cardContext = shuffled(shuffled(shuffled(shuffled(cards))));
      
      this.settings.startedAt = new Date().getTime();


      this.statusMatch = 'match';
    },

    onGetResult() {
      // get timer
      var now = new Date()
      this.timer = new Date(now).getTime() - this.settings.startedAt;

      // switch to result component
      this.statusMatch = 'result';
    }

  },
}
</script>


