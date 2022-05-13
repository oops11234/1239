<template>
  <div id="app">
    <header class="hd">
    <h1 class="hd__title">台灣水庫即時水情</h1>
    </header>
    <main>
      <CardComponent :water-data='waterData[`${damName[0]}`]'></CardComponent>
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
      damName :[],
    }
  },
  created() {
    this.getData();
  },
  computed:{

  },
  methods:{
    getData(){
      const api = 'https://www.taiwanstat.com/waters/latest';
      this.$http.get(api).then(res => {
        this.waterData = res.data[0];
        this.damName =  Object.keys(res.data[0]);
    });
    },
  },
  components: {
    CardComponent,
  },
}
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
</style>
