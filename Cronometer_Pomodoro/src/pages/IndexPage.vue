<template>
  <q-page class="container-principal justify-center column">
    <!-- <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    > -->
    <div class="container-pomodoro justify-center row col">
      <div
        id="cronometro-esquerda"
        class="col row justify-center items-center column"
        @click="toggleTimer('esquerda')"
        :class="{ active: timerActive.esquerda }"
      >
        <transition
          appear
          enter-active-class="animated bounceIn"
          leave-active-class="animated bouceOut"
          :duration="1000"
          >
          <q-circular-progress
            :value="value"
            size="90px"
            :thickness="0.2"
            :class="{
              ligado: timerActive.esquerda,
              hidden: !timerActive.esquerda,
            }"
            center-color="transparent"
            track-color="transparent"
            class="q-ma-md"
          >
          </q-circular-progress>
        </transition>


        {{ formatTime(timerValue.esquerda) }}
      </div>

      <div
        id="cronometro-direita"
        class="col row justify-center items-center column"
        @click="toggleTimer('direita')"
        :class="{ active: timerActive.direita }"
      >
        <q-circular-progress
          :value="value"
          size="90px"
          :thickness="0.2"
          color="orange"
          center-color="transparent"
          track-color="transparent"
          class="q-ma-md"
        >
        </q-circular-progress>

        {{ formatTime(timerValue.direita) }}
      </div>
    </div>

    <div class="q-pa-sm bg-accent">
      <q-btn-group spread color="dark">
        <q-btn
          class="bg-primary"
          @click="resetTimers('L')"
          flat
          color="negative"
          label="Descanso"
        ></q-btn>

        <q-btn
          class="bg-secondary"
          @click="resetTimers('R')"
          flat
          color="positive"
          label="Trabalho"
        ></q-btn>
      </q-btn-group>
    </div>
  </q-page>
</template>

<style>
.ligado {
  color: aqua;
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
      if (lado == "L") {
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

      return `${minutes.toString().padStart(2, "0")}:${seconds
        .toString()
        .padStart(2, "0")}`;
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
      value: 71,
    };
  },
});
</script>
