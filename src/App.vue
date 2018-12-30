<template>
  <div id="app">
    <Scroll :content="list">
      <template slot-scope="{ data }">
        <div
          class="container--item"
          v-for="item in data"
          :key="item">{{ item }}</div>
      </template>
    </Scroll>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Scroll from './components/scroll.vue';

@Component({
  components: {
    Scroll,
  },
})
export default class App extends Vue {
  public list: number[] = [];

  created() {
    let i: number;
    for (i = 0; i < 100000; i += 1) {
      this.list.push(i);
    }
  }
}
</script>

<style>
</style>
