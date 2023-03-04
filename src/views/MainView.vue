<template>
  <div>
    <h1>{{ player }}</h1>
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
    const direction = ref(0);
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

    const moveup = () => {
      if (map.value[player.value[0] - 1][player.value[1]] != 1) {
        map.value[player.value[0]][player.value[1]] = 0;
        player.value[0] = player.value[0] - 1;
        map.value[player.value[0]][player.value[1]] = 2;
      }
      return;
    };

    const movedown = () => {
      if (map.value[player.value[0] + 1][player.value[1]] != 1) {
        map.value[player.value[0]][player.value[1]] = 0;
        player.value[0] = player.value[0] + 1;
        map.value[player.value[0]][player.value[1]] = 2;
      }
      return;
    };

    const moveleft = () => {
      if (map.value[player.value[0]][player.value[1] - 1] != 1) {
        map.value[player.value[0]][player.value[1]] = 0;
        player.value[1] = player.value[1] - 1;
        map.value[player.value[0]][player.value[1]] = 2;
      }
      return;
    };

    const moveright = () => {
      if (map.value[player.value[0]][player.value[1] + 1] != 1) {
        map.value[player.value[0]][player.value[1]] = 0;
        player.value[1] = player.value[1] + 1;
        map.value[player.value[0]][player.value[1]] = 2;
      }
      return;
    };

    document.addEventListener("keydown", (e) => {
      var tecla = e.key;
      if (tecla === "ArrowUp") moveup();
      else if (tecla === "ArrowDown") movedown();
      else if (tecla === "ArrowLeft") moveleft();
      else if (tecla === "ArrowRight") moveright();
    });

    return { player, map, direction, moveup, movedown, moveleft, moveright };
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
