<template>
  <div class="card">
    <div class="card__titleZone">
      <h2 class="card__title">{{ waterData.name }}</h2>
      <div 
        v-if="waterData.percentage >= 50" 
        class="card__content card__content--peace"
        :style="{
            background: `linear-gradient(#fff ${100 - waterData.percentage}%, #0089c5 ${100 - waterData.percentage}%)`,
          }"
        >
        <p class="card__num">{{ netPercentage }} %</p>
      </div>
      <div 
        v-else-if="waterData.percentage >= 30 && waterData.percentage < 50" 
        class="card__content card__content--warning"
        :style="{
            background: `linear-gradient(#fff ${100 - waterData.percentage}%, #ffa077 ${100 - waterData.percentage}%)`,
          }"
        >
        <p class="card__num">{{ netPercentage }} %</p>
      </div>
      <div 
        v-else 
        class="card__content card__content--danger"
        :style="{
            background: `linear-gradient(#fff ${100 - waterData.percentage}%, #ff4444 ${100 - waterData.percentage}%)`,
          }"
        >
        <p class="card__num">{{ netPercentage }} %</p>
      </div>
      <div class="card__text">
        <p class="card__amount">有效蓄水量 : {{ waterData.baseAvailable }} 萬立方公尺</p>
        <p 
          :class="{
            card__netFlow:true,
            'card__netFlow--rise': this.waterData.daliyNetflow > 0,
            'card__netFlow--drop': this.waterData.daliyNetflow < 0,
            }">昨日水量{{ this.waterData.daliyNetflow > 0 ? '上升' : '下降' }} {{ netFlow }}%</p>
        <p class="card__updateAt"> {{ waterData.updateAt }} </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CardComponent',
  props: {
    waterData: [],
  },
  computed:{
     netFlow() {
       if (isNaN(this.waterData.daliyNetflow)){
         return 0;
         }else{
           return Math.abs(+(Math.round(this.waterData.daliyNetflow  + "e+2") + "e-2"));
         }
    },

    netPercentage() {
      return +(Math.round(this.waterData.percentage  + "e+1") + "e-1");
    },
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card {
  padding-right: 20px;
  padding-left: 20px;
  width: 300px;
  box-sizing: border-box;
}

.card__title {
  margin-bottom: 40px;
}

.card__content{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 260px;
  border: 7px solid #fff;
  border-radius: 50%;
}

.card__content--peace {
  color: #0276a8f5;
  outline: 7px solid #0089c5;
}

.card__content--danger {
  color: #c23737b0;
  outline: 7px solid #ff4444;
  background-image: linear-gradient(#fff 30%, #ff4444 30%) 
}

.card__content--warning {
  color: #d38665b9;
  outline: 7px solid #ffa077;
}

.card__num {
  font-size: 64px;
}

.card__text {
  margin-top: 20px;
}

.card__amount {
  margin-bottom: 10px;
  text-align: left;
}

.card__netFlow {
  margin-top: 0px;
  margin-bottom: 0px;
  text-align: left;
}

.card__netFlow--rise {
  color: #0089c5;
}

.card__netFlow--drop {
color: #ff4444;
}

.card__updateAt {
  color: #a8a7a7;
  margin-top: 10px;
  text-align: left;
}

</style>
