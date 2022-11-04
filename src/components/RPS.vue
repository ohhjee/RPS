<template>
  <div>
    <div class="container mx-auto">
      <div class="flex items-center flex-col min-h-screen justify-center">
        <div class="space-x-6">
          <button
            class="ring w-[80px]  h-[80px] md:w-[100px] space-x-6 md:h-[100px] rounded-full  text-[20px] md:text-[1rem]"
            @click="PlayGame('rock')"
            id="all"
            value="Rock"
          >
            Rock
          </button>
          <button
            class="ring w-[80px]  h-[80px] md:w-[100px] space-x-6 md:h-[100px] rounded-full  text-[20px] md:text-[1rem]"
            @click="PlayGame('paper')"
            id="all"
            value="Paper"
          >
            Paper
          </button>
          <button
            class="ring w-[80px]  h-[80px] md:w-[100px] space-x-6 md:h-[100px] rounded-full  text-[20px] md:text-[1rem]"
            @click="PlayGame('scissor')"
            id="all"
            value="Scissor"
          >
            Scissor
          </button>
        </div>

        <div class="flex space-x-3 my-4">
          <div class="text-[1.2rem] md:text-[2rem]">
            <span class="text-black/80">Wins: </span>{{ win }}
          </div>
          <div class="text-[1.2rem] md:text-[2rem]">
            <span class="text-black/80">Lose: </span>{{ lose }}
          </div>
          <div class="text-[1.2rem] md:text-[2rem]">
            <span class="text-black/80">Draw: </span>{{ draw }}
          </div>
        </div>
        <div>
          <div @click="formatScore()" class="cursor-pointer" title="All game data will be lost forever">Format Score</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from "vue";

export default defineComponent({
  setup() {
    onMounted(() => {
      loadgame();
    });

    const computerChoice = ref<string>();
    const choice = ref();
    const win = ref<number|string|any>(0);
    const lose = ref<number|string|any>(0);
    const draw = ref<number|string|any>(0);
    const outcomes = ref({
      rock: {
        rock: "draw",
        scissor: "win",
        paper: "lose",
      },
      paper: {
        rock: "win",
        scissor: "lose",
        paper: "draw",
      },
      scissor: {
        rock: "lose",
        scissor: "draw",
        paper: "win",
      },
    }) as unknown | any;

/**
 * The play function start here
*/
    const PlayGame = (c: unknown) => {
      choice.value = c;
      const games = ref(["rock", "paper", "scissor"]);

      /**
       * Computer choosing randomly
      */
      const random = Math.floor(Math.random() * games.value.length);
      computerChoice.value = games.value[random];
      const outcome = outcomes.value[choice.value][computerChoice.value];
      /**
       * VALIDATE SCORES
       */
      if (outcome === "win") {
        win.value++;
        console.log(outcome);
      } else if (outcome === "lose") {
        lose.value++;
        console.log(outcome);
      } else {
        draw.value++;
        console.log(outcome);
      }
      savegame();
    };
/**
 * Save game score to local storage
*/
    const savegame = () => {
      localStorage.setItem("win", win.value);
      localStorage.setItem("lose", lose.value);
      localStorage.setItem("draw", draw.value);
    };

    /**
     * Load game score from local storage  
    */
    const loadgame = () => {
      win.value = localStorage.getItem("win") || 0;
      lose.value = localStorage.getItem("lose") || 0;
      draw.value = localStorage.getItem("draw") || 0;
    };

    /**
     * Clear score in local storage
    */
    const formatScore = () => {
      win.value =0;
      lose.value = 0;
      draw.value = 0;
      savegame()
  
}
    return { formatScore,PlayGame, win, draw, lose };
  },
});
</script>

<style scoped></style>
