<template>
  <div class="app" id="app">
    <!-- btn help modal -->
    <span class="btn-corner" v-on:click.prevent="showModal('HELP')">&#63;</span>

    <!-- scoreboard -->
    <ul class="scoreboard">
      <li>
        <h4>JOGADOR</h4>
        <h6>{{ this.game.player.score }}</h6>
        <img v-bind:src="playerMove" alt="player_move" />
      </li>
      <li>
        <h4>COMPUTADOR</h4>
        <h6>{{ this.game.pc.score }}</h6>
        <img v-bind:src="pcMove" alt="pc_move" />
      </li>
    </ul>

    <!-- player move controller -->
    <div class="controller">
      <button v-on:click.prevent="play('PEDRA')">PEDRA</button>
      <button v-on:click.prevent="play('PAPEL')">PAPEL</button>
      <button v-on:click.prevent="play('TESOURA')">TESOURA</button>
    </div>

    <!-- help modal -->
    <modal v-show="isHelpVisible" @close="closeModal('HELP')">
      <h1>REGRAS</h1>
      <h2>PEDRA - PAPEL - TESOURA</h2>
      <h6>A pedra ganha à tesoura.</h6>
      <h6>O papel ganha à pedra.</h6>
      <h6>A tesoura ganha ao papel.</h6>
    </modal>

    <!-- result modal -->
    <modal v-show="isResultVisible" @close="closeModal('RESULT')">
      <h1>{{ this.game.result }}</h1>
      <button v-on:click.prevent="closeModal('RESULT')">OUTRO JOGO</button>
    </modal>
  </div>
</template>

<script>
import modal from "./components/modal.vue";

export default {
  name: "App",
  components: {
    modal,
  },
  computed: {
    playerMove() {
      if (this.game.player.move === "PEDRA") {
        return require("./assets/images/PLAYER_PEDRA.png");
      } else if (this.game.player.move === "PAPEL") {
        return require("./assets/images/PLAYER_PAPEL.png");
      } else if (this.game.player.move === "TESOURA") {
        return require("./assets/images/PLAYER_TESOURA.png");
      }

      return require("./assets/images/PLAYER_DEFAULT.png");
    },
    pcMove() {
      if (this.game.pc.move === "PEDRA") {
        return require("./assets/images/PC_PEDRA.png");
      } else if (this.game.pc.move === "PAPEL") {
        return require("./assets/images/PC_PAPEL.png");
      } else if (this.game.pc.move === "TESOURA") {
        return require("./assets/images/PC_TESOURA.png");
      }

      return require("./assets/images/PC_DEFAULT.png");
    },
  },
  data() {
    return {
      game: {
        player: {
          move: "",
          score: 0,
        },
        pc: {
          move: "",
          score: 0,
        },
        result: "",
      },
      isHelpVisible: false,
      isResultVisible: false,
    };
  },
  methods: {
    // close modal
    closeModal(modal) {
      if (modal === "HELP") {
        this.isHelpVisible = false;
      } else if (modal === "RESULT") {
        this.isResultVisible = false;
      }
    },
    // open modal
    showModal(modal) {
      if (modal === "HELP") {
        this.isHelpVisible = true;
      } else if (modal === "RESULT") {
        this.isResultVisible = true;
      }
    },
    // generate pc move
    genPcMove() {
      var moves = ["PEDRA", "PAPEL", "TESOURA"];

      var move = moves[Math.floor(Math.random() * moves.length)];

      this.game.pc.move = move;
    },
    // game play
    play(playerMove) {
      this.genPcMove();

      if (playerMove === this.game.pc.move) {
        this.game.result = "EMPATE";
      } else if (playerMove === "PEDRA" && this.game.pc.move === "PAPEL") {
        this.game.pc.score++;
        this.game.result = "O COMPUTADOR GANHOU!";
      } else if (playerMove === "PEDRA" && this.game.pc.move === "TESOURA") {
        this.game.player.score++;
        this.game.result = "O JOGADOR GANHOU!";
      } else if (playerMove === "PAPEL" && this.game.pc.move === "TESOURA") {
        this.game.pc.score++;
        this.game.result = "O COMPUTADOR GANHOU!";
      } else if (playerMove === "PAPEL" && this.game.pc.move === "PEDRA") {
        this.game.player.score++;
        this.game.result = "O JOGADOR GANHOU!";
      } else if (playerMove === "TESOURA" && this.game.pc.move === "PEDRA") {
        this.game.pc.score++;
        this.game.result = "O COMPUTADOR GANHOU!";
      } else if (playerMove === "TESOURA" && this.game.pc.move === "PAPEL") {
        this.game.player.score++;
        this.game.result = "O JOGADOR GANHOU!";
      }

      this.game.player.move = playerMove;

      this.showModal("RESULT");
    },
  },
};
</script>

<style lang="scss">
@import "./assets/scss/reset.scss";
@import "./assets/scss/style.scss";
</style>
