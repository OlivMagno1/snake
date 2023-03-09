<template>
  <div>
    <div v-show="!playing" class="pause">
      <h2>Pressione a barra de espaço para começar</h2>
    </div>
    <h1>{{ score }}</h1>
    <div v-for="(line, yindex) in map" :key="yindex" class="line">
      <span v-for="(item, xindex) in line" :key="xindex" class="outer place">
        <span class="place" :class="`tipo${item}`" />
      </span>
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
    const speed = ref(400);
    const speedStep = ref(50);
    const score = ref(0);
    var intervalID;

    //Dados do jogador
    const direction = ref(1);
    const lastDirection = ref(1);
    var point = [1, 1];
    var specialPoint = [1, 1];
    let player = [
      [16, 16],
      [15, 16],
      [14, 16],
    ];
    let clearPlayer = [
      [16, 16],
      [15, 16],
      [14, 16],
    ];
    var playerSize = 3;

    function playerNextPos() {
      return Object.assign({}, player[playerSize - 1]);
    }

    function playerLastPos() {
      return Object.assign({}, player[0]);
    }

    //Mapa
    const map = ref([
      [
        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
        1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
      ],
      [
        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
        1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
      ],
    ]);

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
      return pos;
    };

    const addSpecialPosition = () => {
      var pos = getRandomPosition();
      while (map.value[pos[1]][pos[0]] != 0) pos = getRandomPosition();
      map.value[pos[1]][pos[0]] = 4;
      return pos;
    };

    //Jogador coletou um item
    const getPoint = () => {
      score.value = score.value + 1;
      point = addPosition();
      player[playerSize] = Object.assign({}, player[playerSize - 1]);
      playerSize = playerSize + 1;
      if (speed.value >= 70) {
        speed.value = speed.value - speedStep.value;
        if (speedStep.value > 5) speedStep.value = speedStep.value - 5;
      }
      if (score.value % 5 == 0) {
        specialPoint = addSpecialPosition();
        setTimeout(function () {
          map.value[specialPoint[1]][specialPoint[0]] = 0;
          specialPoint = [1, 1];
        }, 5000);
      }
      return;
    };

    const getSpecialPoint = () => {
      score.value = score.value + 5;
      player[playerSize] = Object.assign({}, player[playerSize - 1]);
      playerSize = playerSize + 1;
    };

    //Reseta o estado do jogo, deveria funcionar todas as vezes, mas algo não está certo
    const reset = () => {
      if (playing.value == true) {
        clearTimeout(intervalID);
        //Limpeza das configurações
        playing.value = false;
        score.value = 0;
        direction.value = 1;
        speed.value = 400;
        speedStep.value = 50;
        //Limpeza do mapa
        for (var j = 0; j < playerSize; j++) {
          map.value[player[j][0]][player[j][1]] = 0;
        }
        map.value[point[1]][point[0]] = 0;
        map.value[specialPoint[1]][specialPoint[0]] = 0;
        player = Object.assign({}, clearPlayer);
        playerSize = 3;
        return;
      }
    };

    //Começa o jogo
    const begin = () => {
      if (playing.value == false) {
        playing.value = true;
        point = addPosition();
        moveOuter();
      }
    };

    const moveInner = () => {
      if (playing.value == true) {
        intervalID = setTimeout(function () {
          moveOuter();
        }, speed.value);
      }
    };

    const moveOuter = () => {
      if (direction.value == 1) {
        move(-1, 0, moveInner);
        lastDirection.value = 1;
      } else if (direction.value == 2) {
        move(0, 1, moveInner);
        lastDirection.value = 2;
      } else if (direction.value == 3) {
        move(1, 0, moveInner);
        lastDirection.value = 3;
      } else if (direction.value == 4) {
        move(0, -1, moveInner);
        lastDirection.value = 4;
      }
    };

    const move = (y, x, callback) => {
      let nextPos = playerNextPos();
      let lastPos = playerLastPos();
      nextPos[0] = nextPos[0] + y;
      nextPos[1] = nextPos[1] + x;
      if (
        map.value[nextPos[0]][nextPos[1]] != 1 &&
        map.value[nextPos[0]][nextPos[1]] != 2
      ) {
        if (map.value[nextPos[0]][nextPos[1]] == 3) {
          getPoint();
        } else if (map.value[nextPos[0]][nextPos[1]] == 4) {
          getSpecialPoint();
        }
        for (var i = 1; i < playerSize; i++) {
          player[i - 1] = Object.assign({}, player[i]);
        }
        player[playerSize - 1] = Object.assign({}, nextPos);
        map.value[lastPos[0]][lastPos[1]] = 0;
        map.value[nextPos[0]][nextPos[1]] = 2;
      } else {
        //atingiu uma parede
        reset();
      }
      callback();
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
      speed,
      speedStep,
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
      moveOuter,
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

p {
  color: white;
}

.line {
  display: flex;
  flex-flow: row nowrap;
}

.outer {
  background-color: var(--empty);
}

.place {
  display: flex;
  flex-flow: row nowrap;
  width: 1.2rem;
  height: 1.2rem;
}

.tipo1,
.tipo2 {
  background-color: var(--filled);
}

.tipo0 {
  background-color: var(--empty);
}

.tipo3 {
  border-radius: 0.8rem;
  background-color: var(--fruit);
  transition: 0.5s;
}

.tipo4 {
  border-radius: 0.8rem;
  background-color: var(--specialFruit);
  transition: 0.5s;
}
</style>
