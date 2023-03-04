<template>
  <div>
    <h1>{{ score }}</h1>
    <div v-for="(line, yindex) in map" :key="yindex" class="line">
      <span
        v-for="(item, xindex) in line"
        :key="xindex"
        class="place"
        :class="`tipo${item}`"
      />
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  name: "MainView",
  setup() {
    //Configurações de funcionamento
    const playing = ref(0);
    const speed = ref(1000);
    var intervalID;

    //Dados do jogador
    const score = ref(0);
    const direction = ref(1);
    const player = ref([20, 5]);
    const map = ref([
      [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
      [1, 0, 0, 0, 0, 2, 0, 0, 0, 0, 1],
      [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
    ]);

    const reset = () => {
      clearInterval(intervalID);
      score.value = 0;
      playing.value = 0;
      map.value[player.value[0]][player.value[1]] = 0;
      player.value = [20, 5];
      map.value[player.value[0]][player.value[1]] = 2;
      direction.value = 1;
    };

    const begin = () => {
      playing.value = 1;
      intervalID = setInterval(function () {
        if (playing.value == 1) move();
      }, speed.value);
    };

    const move = () => {
      score.value = score.value + 1;
      if (direction.value == 1) moveup();
      else if (direction.value == 2) moveright();
      else if (direction.value == 3) movedown();
      else if (direction.value == 4) moveleft();
    };

    const moveup = () => {
      if (map.value[player.value[0] - 1][player.value[1]] != 1) {
        map.value[player.value[0]][player.value[1]] = 0;
        player.value[0] = player.value[0] - 1;
        map.value[player.value[0]][player.value[1]] = 2;
      } else {
        console.log("Fim de jogo");
        reset();
      }
      return;
    };

    const movedown = () => {
      if (map.value[player.value[0] + 1][player.value[1]] != 1) {
        map.value[player.value[0]][player.value[1]] = 0;
        player.value[0] = player.value[0] + 1;
        map.value[player.value[0]][player.value[1]] = 2;
      } else {
        console.log("Fim de jogo");
        reset();
      }
      return;
    };

    const moveleft = () => {
      if (map.value[player.value[0]][player.value[1] - 1] != 1) {
        map.value[player.value[0]][player.value[1]] = 0;
        player.value[1] = player.value[1] - 1;
        map.value[player.value[0]][player.value[1]] = 2;
      } else {
        console.log("Fim de jogo");
        reset();
      }
      return;
    };

    const moveright = () => {
      if (map.value[player.value[0]][player.value[1] + 1] != 1) {
        map.value[player.value[0]][player.value[1]] = 0;
        player.value[1] = player.value[1] + 1;
        map.value[player.value[0]][player.value[1]] = 2;
      } else {
        console.log("Fim de jogo");
        reset();
      }
      return;
    };

    document.addEventListener("keydown", (e) => {
      var tecla = e.key;
      if (tecla === " ") begin();
      else if (tecla === "Enter") playing.value = 0;
      else if (tecla === "ArrowUp") direction.value = 1;
      else if (tecla === "ArrowRight") direction.value = 2;
      else if (tecla === "ArrowDown") direction.value = 3;
      else if (tecla === "ArrowLeft") direction.value = 4;
    });

    return {
      score,
      player,
      map,
      direction,
      intervalID,
      reset,
      begin,
      move,
      moveup,
      movedown,
      moveleft,
      moveright,
    };
  },
};
</script>

<style scoped>
.line {
  display: flex;
  flex-flow: row nowrap;
}

.place {
  width: 1.8rem;
  height: 1.8rem;
}

.tipo1,
.tipo2 {
  background-color: var(--filled);
}

.tipo0 {
  background-color: var(--empty);
}
</style>
