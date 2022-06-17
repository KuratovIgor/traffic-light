<template>
  <div>
    <div class="traffic-light">
      <div class="traffic-light__auto">
        <div class="circle" :class="isRed ? 'circle--red' : ''" >
          <div v-if="redTimer">{{ redTimer }}</div>
        </div>
        <div class="circle" :class="isYellow ? 'circle--yellow' : ''" />
        <div class="circle" :class="isGreen ? 'circle--green' : ''">
          <div v-if="greenTimer">{{ greenTimer }}</div>
        </div>
      </div>

      <div class="traffic-light__human">
        <div v-show="!isCanCross" class="circle circle--red" />
        <div v-show="redTimer">{{ redTimer }}</div>

        <div v-show="isCanCross" class="circle circle--green" />
        <div v-show="greenTimer">{{ greenTimer }}</div>
      </div>

      <button @click="handleCrossRoad">Я хочу перейти дорогу!</button>
    </div>
  </div>
</template>

<script>
import {reactive, toRefs} from "vue";

export default {
  name: 'App',

  setup() {
    const data = reactive({
      isRed: false,
      isYellow: false,
      isGreen: true,
      isCanCross: false,
      greenTimer: 0,
      redTimer: 0,
    })

    const handleCrossRoad = () => {
      showGreen().then(() => {
        showYellow().then(() => {
          data.isCanCross = true

          showRed().then(() => {
            data.isCanCross = false

            showYellow().then(() => {
              data.isGreen = true
            })
          })
        })
      })
    }

    const showGreen = () => {
      data.greenTimer = 10

      return new Promise((resolve) => {
        const id = setInterval(() => {
          data.greenTimer--

          if (data.greenTimer === 0) {
            data.isGreen = false
            clearTimeout(id)
            resolve()
          }
        }, 1000)
      })
    }

    const showYellow = () => {
      data.isYellow = true

      return new Promise((resolve) => {
        setTimeout(() => {
          data.isYellow = false
          resolve()
        }, 2000)
      })
    }

    const showRed = () => {
      data.redTimer = 20
      data.isRed = true

      return new Promise((resolve) => {
        const id = setInterval(() => {
          data.redTimer--

          if (data.redTimer === 0) {
            data.isRed = false
            clearTimeout(id)
            resolve()
          }
        }, 1000)
      })
    }



    return {
      ...toRefs(data),
      handleCrossRoad,
    }
  }
}
</script>

<style scoped>
@font-face {
  font-family: 'Digital';
  src: url('assets/digital.ttf');
}

div {
  font-size: 30px;
  font-family: 'Digital';

}

.traffic-light {
  max-width: 500px;
  display: flex;
  justify-content: space-between;
}

.traffic-light__auto {
  border: 2px solid #000;
  width: 150px;
  height: 400px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  background: antiquewhite;
}

.traffic-light__human {
  border: 2px solid #000;
  width: 150px;
  height: 400px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  background: antiquewhite;
}

.circle {
  width: 100px;
  height: 100px;
  border: 2px solid #000;
  border-radius: 50%;
  background: lightblue;
  display: flex;
  justify-content: center;
  align-items: center;
}

.circle--red {
  background: red;
}

.circle--yellow {
  background: yellow;
}

.circle--green {
  background: green;
}
</style>
