<template>
  <div>
    <div v-show="!playing" class="pause">
      <h2>Pressione a barra de espaço para começar</h2>
    </div>
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
    const playing = ref(false);
    const speed = ref(750);
    var intervalID;

    //Dados do jogador
    const score = ref(0);
    const direction = ref(1);
    const lastDirection = ref(1);
    const player = ref([20, 5]);

    //Mapa
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

    //Backup do mapa limpo
    const clearMap = [
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
    ];

    //Gera uma posição aleatória no mapa
    var getRandomPosition = () => {
      var posX = Math.floor(Math.random() * 9) + 1;
      var posY = Math.floor(Math.random() * 20) + 1;
      var newPos = [posX, posY];
      return newPos;
    };

    //Cria um item na posição aleatória gerada
    const addPosition = () => {
      var pos = getRandomPosition();
      while (map.value[pos[1]][pos[0]] != 0) pos = getRandomPosition();
      map.value[pos[1]][pos[0]] = 3;
    };

    //Jogador coletou um item
    const getPoint = () => {
      score.value = score.value + 1;
      addPosition();
    };

    //Reseta o estado do jogo, deveria funcionar todas as vezes, mas algo não está certo
    const reset = () => {
      clearInterval(intervalID);
      score.value = 0;
      playing.value = false;
      map.value = clearMap;
      player.value = [20, 5];
      direction.value = 1;
    };

    //Começa o jogo
    const begin = () => {
      if (playing.value == false) {
        addPosition();
        playing.value = true;
        intervalID = setInterval(function () {
          move();
        }, speed.value);
      }
    };

    const move = () => {
      if (direction.value == 1) {
        moveup();
        lastDirection.value = 1;
      } else if (direction.value == 2) {
        moveright();
        lastDirection.value = 2;
      } else if (direction.value == 3) {
        movedown();
        lastDirection.value = 3;
      } else if (direction.value == 4) {
        moveleft();
        lastDirection.value = 4;
      }
    };

    const moveup = () => {
      if (map.value[player.value[0] - 1][player.value[1]] != 1) {
        //coletar item e adicionar novo item no mapa
        if (map.value[player.value[0] - 1][player.value[1]] == 3) {
          getPoint();
        }
        //movimentação
        map.value[player.value[0]][player.value[1]] = 0;
        player.value[0] = player.value[0] - 1;
        map.value[player.value[0]][player.value[1]] = 2;
      } else {
        //atingiu uma parede
        console.log("Fim de jogo");
        reset();
      }
      return;
    };

    const movedown = () => {
      if (map.value[player.value[0] + 1][player.value[1]] != 1) {
        //coletar item e adicionar novo item no mapa
        if (map.value[player.value[0] + 1][player.value[1]] == 3) {
          getPoint();
        }
        //movimentação
        map.value[player.value[0]][player.value[1]] = 0;
        player.value[0] = player.value[0] + 1;
        map.value[player.value[0]][player.value[1]] = 2;
      } else {
        //atingiu uma parede
        console.log("Fim de jogo");
        reset();
      }
      return;
    };

    const moveleft = () => {
      if (map.value[player.value[0]][player.value[1] - 1] != 1) {
        //coletar item e adicionar novo item no mapa
        if (map.value[player.value[0]][player.value[1] - 1] == 3) {
          getPoint();
        }
        //movimentação
        map.value[player.value[0]][player.value[1]] = 0;
        player.value[1] = player.value[1] - 1;
        map.value[player.value[0]][player.value[1]] = 2;
      } else {
        //atingiu uma parede
        console.log("Fim de jogo");
        reset();
      }
      return;
    };

    const moveright = () => {
      if (map.value[player.value[0]][player.value[1] + 1] != 1) {
        //coletar item e adicionar novo item no mapa
        if (map.value[player.value[0]][player.value[1] + 1] == 3) {
          getPoint();
        }
        //movimentação
        map.value[player.value[0]][player.value[1]] = 0;
        player.value[1] = player.value[1] + 1;
        map.value[player.value[0]][player.value[1]] = 2;
      } else {
        //atingiu uma parede
        console.log("Fim de jogo");
        reset();
      }
      return;
    };

    document.addEventListener("keydown", (e) => {
      var tecla = e.key;
      if (tecla === " ") begin();
      else if (tecla === "Enter" && playing.value == true) reset();
      else if (tecla === "ArrowUp" && lastDirection.value != 3)
        direction.value = 1;
      else if (tecla === "ArrowRight" && lastDirection.value != 4)
        direction.value = 2;
      else if (tecla === "ArrowDown" && lastDirection.value != 1)
        direction.value = 3;
      else if (tecla === "ArrowLeft" && lastDirection.value != 2)
        direction.value = 4;
    });

    return {
      score,
      player,
      map,
      direction,
      lastDirection,
      intervalID,
      playing,
      getRandomPosition,
      addPosition,
      getPoint,
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
.pause {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;

  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 150;
  backdrop-filter: blur(4px);
  transition: 0.5s;
}

h1 {
  color: var(--empty);
}

h2 {
  color: white;
}

.line {
  display: flex;
  flex-flow: row nowrap;
}

.place {
  width: 1.8rem;
  height: 1.8rem;
  transition: 0.5s;
}

.tipo1,
.tipo2 {
  background-color: var(--filled);
}

.tipo0 {
  background-color: var(--empty);
}
</style>
