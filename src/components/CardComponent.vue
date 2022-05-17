<template>
  <div class="card">
    <h2 class="card__title">{{ parentData.name }}</h2>
    <div
      :class="[
        'card__content',
        {
          'card__content--peace': parentData.percentage >= 50,
          'card__content--warning':
            parentData.percentage >= 30 && parentData.percentage < 50,
          'card__content--danger': parentData.percentage < 30,
        },
      ]"
    >
      <div class="card__num">
        <div
          class="card__bg"
          :style="{ height: `${parentData.percentage}%` }"
        ></div>
        <p class="card__desc">{{ netPercentage }}%</p>
      </div>
    </div>
    <div class="card__text">
      <p class="card__amount">有效蓄水量 : {{ parentData.volumn }} 萬立方公尺</p>
      <p
        :class="[
          'card__netFlow',
          {
            'card__netFlow--rise': this.parentData.daliyNetflow <= 0,
            'card__netFlow--drop': this.parentData.daliyNetflow > 0,
          },
        ]"
      >
        昨日水量{{ statusText() }}
        {{ countNetFlow}}
      </p>
      <p class="card__updateAt">{{ parentData.updateAt }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CardComponent',
  props: {
    parentData: {
      type: Object,
    },
  },
  computed: {
    countNetFlow() {
      if (isNaN(this.parentData.daliyNetflow)) {
        return '待更新';
      } else {
        return (
          ((Math.abs(this.parentData.daliyNetflow) / this.parentData.baseAvailable) * 100).toFixed(2) + '%');
      }
    },

    netPercentage() {
      return this.parentData.percentage.toFixed(1);
    },
  },
  methods: {
    statusText() {
      return this.parentData.daliyNetflow >= 0
        ? '下降'
        : this.parentData.daliyNetflow < 0
          ? '上升'
          : '狀態';
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card {
  box-sizing: border-box;
}

.card__title {
  margin-bottom: 40px;
}

.card__content {
  position: relative;
  height: 270px;
  border-radius: 50%;
  box-sizing: border-box;
}

.card__content--peace {
  color: #0089c5;
  border: 7px solid #0089c5;
}

.card__content--warning {
  color: #ffa077;
  border: 7px solid #ffa077;
}

.card__content--danger {
  color: #ff4444;
  border: 7px solid #ff4444;
}

.card__num {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  font-size: 64px;
  box-sizing: border-box;
  border: 7px solid #fff;
  border-radius: 50%;
  overflow: hidden;
}

.card__desc {
  mix-blend-mode: multiply;
}

.card__bg {
  position: absolute;
  bottom: 0;
  width: 100%;
}

.card__content--peace .card__bg {
  background-color: #0089c5;
}

.card__content--warning .card__bg {
  background-color: #ffa077;
}

.card__content--danger .card__bg {
  background-color: #ff4444;
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
  margin-top: 10px;
  color: #a8a7a7;
  text-align: left;
}
</style>
