<template>
  <div>
    <div v-show="!playing" class="pause">
      <h1>{{ text }}</h1>
    </div>
    <div v-show="playing">
      <div class="scorePlace">
        <h2>{{ score }}</h2>
        <h3 v-show="specialSpawned">+{{ specialValue }}</h3>
      </div>
      <div v-for="(line, yindex) in map" :key="yindex" class="line">
        <span
          v-for="(item, xindex) in line"
          :key="xindex"
          class="place"
          :class="`tipo${item}`"
        >
        </span>
      </div>
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
    const text = ref("Pressione a barra de espaço para começar");
    let touchstartX = 0;
    let touchendX = 0;
    let touchstartY = 0;
    let touchendY = 0;
    let touchY = 0;
    let touchX = 0;
    var intervalID;
    var intervalSID;

    //Dados do jogador
    const direction = ref(1);
    const lastDirection = ref(1);
    var point = [1, 1];
    const specialSpawned = ref(false);
    var specialPoint = [1, 1];
    const specialValue = ref(15);
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

    //prettier-ignore
    const map = ref([
      ["nw","n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "n", "ne"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["w", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "0", "e"],
      ["sw", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "s", "se"],
    ]);

    //Gera uma posição aleatória no mapa
    var getRandomPosition = () => {
      var posX = Math.floor(Math.random() * 32) + 1;
      var posY = Math.floor(Math.random() * 32) + 1;
      var newPos = [posX, posY];
      return newPos;
    };

    //Cria um item na posição aleatória gerada
    const addPosition = () => {
      var pos = getRandomPosition();
      while (map.value[pos[1]][pos[0]] != "0") pos = getRandomPosition();
      map.value[pos[1]][pos[0]] = 3;
      return pos;
    };

    const addSpecialPosition = () => {
      specialSpawned.value = true;
      specialValue.value = 15;
      var pos = getRandomPosition();
      while (map.value[pos[1]][pos[0]] != "0") pos = getRandomPosition();
      map.value[pos[1]][pos[0]] = 4;
      return pos;
    };

    const specialLifeDecay = () => {
      if (specialValue.value > 1) specialValue.value = specialValue.value - 1;
      else specialSpawned.value = false;
      if (specialSpawned.value == true)
        intervalSID = setTimeout(function () {
          specialLifeDecay();
        }, 1000);
      else {
        if (map.value[specialPoint[1]][specialPoint[0]] == 4)
          map.value[specialPoint[1]][specialPoint[0]] = "0";
        specialPoint = [1, 1];
        clearTimeout(intervalSID);
      }
    };

    const changePalette = () => {
      var doc = getComputedStyle(document.documentElement);
      if (doc.getPropertyValue("--filled") == " #02ffe1") {
        document.documentElement.style.setProperty("--filled", " #00ff28");
      } else if (doc.getPropertyValue("--filled") == " #00ff28") {
        document.documentElement.style.setProperty("--filled", " #fffc02");
      } else if (doc.getPropertyValue("--filled") == " #fffc02") {
        document.documentElement.style.setProperty("--filled", " #ff7b02");
      } else if (doc.getPropertyValue("--filled") == " #ff7b02") {
        document.documentElement.style.setProperty("--filled", " #ff0202");
      } else if (doc.getPropertyValue("--filled") == " #ff0202") {
        document.documentElement.style.setProperty("--filled", " #ff0063");
      } else if (doc.getPropertyValue("--filled") == " #ff0063") {
        document.documentElement.style.setProperty("--filled", " #cc00ff");
      } else if (doc.getPropertyValue("--filled") == " #cc00ff") {
        document.documentElement.style.setProperty("--filled", " #000aff");
      } else if (doc.getPropertyValue("--filled") == " #000aff") {
        document.documentElement.style.setProperty("--filled", " #02ffe1");
      }
    };

    //Jogador coletou um item
    const getPoint = () => {
      score.value = score.value + 1;
      point = addPosition();
      player[playerSize] = Object.assign({}, player[playerSize - 1]);
      playerSize = playerSize + 1;
      changePalette();
      if (speed.value >= 70) {
        speed.value = speed.value - speedStep.value;
        if (speedStep.value > 5) speedStep.value = speedStep.value - 5;
      }
      if (score.value % 5 == 0) {
        specialPoint = addSpecialPosition();
        specialLifeDecay();
      }
      return;
    };

    const getSpecialPoint = () => {
      score.value = score.value + specialValue.value;
      player[playerSize] = Object.assign({}, player[playerSize - 1]);
      playerSize = playerSize + 1;
      specialValue.value = 15;
      specialSpawned.value = false;
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
        text.value = "Fim de jogo";
        setTimeout(function () {
          text.value = "Pressione a barra de espaço para começar";
        }, 1000);
        //Limpeza do mapa
        document.documentElement.style.setProperty("--filled", " #02ffe1");
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
    const beginInner = () => {
      playing.value = true;
      point = addPosition();
      moveOuter();
    };

    const beginOuter = (callback) => {
      if (playing.value == false) {
        setTimeout(function () {
          text.value = "3";
          setTimeout(function () {
            text.value = "2";
            setTimeout(function () {
              text.value = "1";
              setTimeout(function () {
                callback();
              }, 300);
            }, 300);
          }, 300);
        }, 10);
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
        map.value[nextPos[0]][nextPos[1]] != "n" &&
        map.value[nextPos[0]][nextPos[1]] != "s" &&
        map.value[nextPos[0]][nextPos[1]] != "e" &&
        map.value[nextPos[0]][nextPos[1]] != "w" &&
        map.value[nextPos[0]][nextPos[1]] != "2"
      ) {
        if (map.value[nextPos[0]][nextPos[1]] == "3") {
          getPoint();
        } else if (map.value[nextPos[0]][nextPos[1]] == "4") {
          getSpecialPoint();
        }
        for (var i = 1; i < playerSize; i++) {
          player[i - 1] = Object.assign({}, player[i]);
        }
        player[playerSize - 1] = Object.assign({}, nextPos);
        map.value[lastPos[0]][lastPos[1]] = "0";
        map.value[nextPos[0]][nextPos[1]] = "2";
      } else {
        //atingiu uma parede
        reset();
      }
      callback();
      return;
    };

    //keyboard control
    document.addEventListener("keydown", (e) => {
      var tecla = e.key;
      if (tecla === " ") beginOuter(beginInner);
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

    //touch control
    function checkDirection() {
      touchX = touchendX - touchstartX;
      touchY = touchendY - touchstartY;
      //horizontal move
      if (Math.abs(touchX) > Math.abs(touchY)) {
        if (touchX > 0 && lastDirection.value != 4) direction.value = 2;
        else if (touchX < 0 && lastDirection.value != 2) direction.value = 4;
      } else {
        if (touchY < 0 && lastDirection.value != 3) direction.value = 1;
        else if (touchY > 0 && lastDirection.value != 1) direction.value = 3;
      }
    }

    document.addEventListener("touchend", () => {
      if (playing.value == false) beginOuter(beginInner);
    });

    document.addEventListener("touchstart", (e) => {
      touchstartX = e.changedTouches[0].screenX;
      touchstartY = e.changedTouches[0].screenY;
    });

    document.addEventListener("touchend", (e) => {
      touchendX = e.changedTouches[0].screenX;
      touchendY = e.changedTouches[0].screenY;
      checkDirection();
    });

    return {
      text,
      score,
      speed,
      speedStep,
      specialValue,
      specialSpawned,
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
      beginOuter,
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
  transition: 0.5s;
}

.scorePlace {
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  z-index: 50;
  width: clamp(17rem, 1.133rem + 79.3vw, 40.8rem);
  height: clamp(17rem, 1.133rem + 79.3vw, 40.8rem);
}

h1 {
  color: white;
  filter: drop-shadow(0 0 5px var(--filled));
  font-family: TiltNeon;
}

h2 {
  font-size: clamp(6.8rem, 0.453rem + 31.7vw, 16.3rem);
  opacity: 0.2;
  color: var(--filled);
  filter: drop-shadow(0 0 20px var(--filled));
  font-family: TiltNeon;
}

h3 {
  position: absolute;
  right: clamp(1.5rem, 0.1rem + 7vw, 3.6rem);
  bottom: clamp(5.5rem, 0.367rem + 25.7vw, 13.2rem);
  font-size: clamp(2rem, 0.067rem + 9.67vw, 4.9rem);
  opacity: 0.2;
  color: var(--fruit);
  filter: drop-shadow(0 0 10px var(--filled));
  font-family: TiltNeon;
}

p {
  color: white;
  font-family: TiltNeon;
}

.line {
  display: flex;
  flex-flow: row nowrap;
}

.place {
  display: flex;
  flex-flow: row nowrap;
  width: clamp(0.5rem, 0.033rem + 2.3vw, 1.2rem);
  height: clamp(0.5rem, 0.033rem + 2.3vw, 1.2rem);
}

.tipon,
.tipos,
.tipow,
.tipoe,
.tiponw,
.tiposw,
.tipone,
.tipose,
.tipo2 {
  background-color: var(--filled);
  filter: drop-shadow(0 0 5px var(--filled));
}

.tiponw {
  border-radius: 1rem 0 0 0;
}

.tipone {
  border-radius: 0 1rem 0 0;
}

.tiposw {
  border-radius: 0 0 0 1rem;
}

.tipose {
  border-radius: 0 0 1rem 0;
}

.tipo0 {
  background-color: var(--background);
}

.tipo3 {
  border-radius: 0.8rem;
  background-color: var(--filled);
  filter: drop-shadow(0 0 10px var(--filled));
  transition: 0.5s;
}

.tipo4 {
  border-radius: 0.8rem;
  background-color: var(--fruit);
  filter: drop-shadow(0 0 10px var(--filled));
  transition: 0.5s;
}
</style>
