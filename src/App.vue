<template>
  <div class="container mx-auto" id="app">
    <header class="hd">
      <h1 class="hd__title">台灣水庫即時水情</h1>
    </header>
    <main class="main grid">
      <CardComponent
        v-for="item in waterData"
        :key="item.id"
        :parent-data="item"
      />
    </main>
  </div>
</template>

<script>
import CardComponent from './components/CardComponent.vue';

export default {
  name: 'App',
  data() {
    return {
      waterData: [],
    };
  },

  async created() {
    const res = await this.axios({
      url: 'https://www.taiwanstat.com/waters/latest',
      method: 'GET',
    });
    this.waterData = res.data[0];
  },

  components: {
    CardComponent,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  width: 1200px;
}

.mx-auto {
  margin-right: auto;
  margin-left: auto;
}

.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 30px;
}
</style>
