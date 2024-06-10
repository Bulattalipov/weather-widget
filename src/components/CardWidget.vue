<script>
import dayjs from 'dayjs';
export default {
  data() {
    return {
      dateDayWeek: '',
      dateDay: 0,
      dateMonth: '',
      gradColor: '',
    };
  },
  props: {
    temp: {
      type: Number,
      required: true,
    },
    name: {
      type: String,
      required: true,
    },
    description: {
      type: String,
      required: true,
    },
    tempMin: {
      type: Number,
      required: true,
    },
    tempMax: {
      type: Number,
      required: true,
    },
    icon: {
      type: String,
      required: true,
    },
    imgUrl: {
      type: String,
      required: true,
    },
  },
  methods: {
    getDate() {
      const c = dayjs();

      this.dateDayWeek = c.startOf('week').format('dddd');
      this.dateDay = c.date();
      this.dateMonth = c.startOf('month').format('MMM');
    },
    identificationСolor(temp) {
      if (temp >= 30) {
        this.gradColor =
          'background: linear-gradient(271deg, #F94545 0%, #F94545 0.01%, rgba(222, 194, 123, 0.6) 100%), linear-gradient(307deg, rgba(239, 188, 56, 0.2) 0%, rgba(250, 186, 24, 0.2) 100%);';
      } else if (temp < 30 && temp > 0) {
        this.gradColor =
          'background: linear-gradient(271deg, #fdd368 0%, #f8b70f 0.01%, rgba(222, 194, 123, 0.6) 100%), linear-gradient(307deg, rgba(239, 188, 56, 0.2) 0%, rgba(250, 186, 24, 0.2) 100%)';
      } else {
        this.gradColor =
          'background: linear-gradient(271deg, #3768A3 0%, #3768A3 0.01%, rgba(222, 194, 123, 0.6) 100%), linear-gradient(307deg, rgba(239, 188, 56, 0.2) 0%, rgba(250, 186, 24, 0.2) 100%);';
      }
    },
  },
  mounted() {
    this.getDate();
    this.identificationСolor(this.temp);
  },
};
</script>

<template>
  <div class="widget" :style="gradColor">
    <img class="widget__img" :src="imgUrl" alt="Kazan" />
    <div class="widget__inner">
      <div class="widget__top">
        <div class="widget__temp">{{ temp }}°</div>
        <div class="widget__location">{{ name }}</div>
      </div>
      <div class="widget__box">
        <div class="widget__box-inner">
          <div class="widget__date">{{ dateDayWeek }}, {{ dateDay }} {{ dateMonth }}</div>
          <div class="widget__state">
            <div class="widget__state-sky">{{ description }}</div>
            <div class="widget__state-temp">{{ tempMin }}° / {{ tempMax }}°</div>
          </div>
        </div>
        <img class="widget__icon" :src="`https://openweathermap.org/img/wn/${icon}.png`" alt="" />
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.widget {
  position: relative;
  border-radius: 16px;
  width: 243px;
  height: 332px;
  margin: auto;
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.25);
  overflow: hidden;

  &__img {
    position: absolute;
    height: 100%;
    width: 100%;
    z-index: -1;
  }

  &__inner {
    min-height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }

  &__top {
    margin-top: 24px;
    margin-left: 23px;
  }

  &__temp {
    font-weight: 600;
    font-size: 40px;
    letter-spacing: -0.02em;
    color: #fff;
  }

  &__location {
    font-weight: 700;
    letter-spacing: -0.02em;
    color: #fff;
  }

  &__box {
    display: flex;
    align-items: flex-end;
    justify-content: space-between;
    min-height: 93px;
    background-color: #fff;
    padding: 16px;
  }

  &__date {
    display: flex;
    color: #191919;
    opacity: 0.7;
    font-weight: 700;
    margin-bottom: 7px;
  }

  &__state {
    font-size: 12px;
    color: #6b6b6b;
    opacity: 0.4;
  }
}
</style>
