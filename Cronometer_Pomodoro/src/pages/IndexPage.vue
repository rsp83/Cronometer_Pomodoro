<template>
  <q-page class="container-principal">
    <!-- <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    > -->
    <div class="container-pomodoro">
      <div id="cronometro-esquerda"  @click="toggleTimer('esquerda')" :class="{ 'active': timerActive.esquerda }">
        {{ formatTime(timerValue.esquerda) }}
      </div>
      <div id="cronometro-direita" @click="toggleTimer('direita')" :class="{ 'active': timerActive.direita }">
        {{ formatTime(timerValue.direita) }}
      </div>
    </div>
    <div class="container-resetar">
      <button @click="resetTimers('L')">RESET L</button>
      <button @click="resetTimers('R')">RESET R</button>

    </div>
  </q-page>
</template>

<style>

.container-principal{
  display: flex;
  width: 100%;
  height: 100vh;
  max-height: 100%;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  font-size: 30px;
}

.container-pomodoro{
  display: flex;
  width: 100%;
  height: 90%;
  align-items: center;
  justify-content: center;
  background-color: rgb(255, 62, 94);
}

.container-pomodoro div{
  display: flex;
  width: 50%;
  background-color: blue;
  justify-content: center;
  height: 100%;
  align-items: center;
}


.container-pomodoro div:nth-child(1){
  background-color: rgb(0, 255, 128);
}

.container-resetar{
  display: flex;
  width: 100%;
  height: 10%;
  justify-content: center;
  align-items: center;
  background-color: rgb(0, 0, 0);
}

.container-resetar button{
  padding-right:55px;
  padding-left:55px;

  height: 100%;
  box-sizing: border-box;
}

#cronometro-esquerda{
  background-color: rgb(105, 105, 105);
  /* border-right: 55px solid black; */
}

#cronometro-esquerda:hover{
  background-color: rgb(132, 113, 134);

}
#cronometro-direita{
  background-color: rgb(105, 105, 105);
  /* border-left: 55px solid black; */
}
#cronometro-direita:hover{
  background-color: rgb(146, 140, 130);

}
</style>

<script>
import { defineComponent, ref, watch } from "vue";

export default defineComponent({
  name: "IndexPage",
  setup() {
    const timerValue = ref({
      esquerda: 0,
      direita: 0,
    });

    const timerActive = ref({
      esquerda: false,
      direita: false,
    });

    const timers = {
      esquerda: null,
      direita: null,
    };

    const toggleTimer = (lado) => {
      timerActive.value[lado] = !timerActive.value[lado];

      if (timerActive.value[lado]) {
        timers[lado] = setInterval(() => {
          timerValue.value[lado]++;
        }, 1000); // Atualiza a cada segundo (1000 milissegundos)
      } else {
        clearInterval(timers[lado]);
      }
    };

    const resetTimers = (lado) => {

      if(lado == 'L'){
        timerValue.value.esquerda = 0;
        timerActive.value.esquerda = false;
        clearInterval(timers.esquerda);

      } else {
        timerValue.value.direita = 0;
        timerActive.value.direita = false;
        clearInterval(timers.direita);
      }

    };

    const formatTime = (value) => {
      const minutes = Math.floor(value / 60);
      const seconds = value % 60;

      return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
    };

    // Reinicie os timers quando o componente é destruído
    watch(timerValue, () => {
      if (!timerActive.value.esquerda && !timerActive.value.direita) {
        clearInterval(timers.esquerda);
        clearInterval(timers.direita);
      }
    });

    return {
      timerValue,
      timerActive,
      toggleTimer,
      resetTimers,
      formatTime,
    };
  },
});


</script>
