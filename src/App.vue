<template>
  <b-card
    class="mt-4 ml-5 mr-5 mb-4"
    style="background-color: rgba(255, 255, 255, 0.8) !important"
    title="Chess Test Vue Js version!"
    :sub-title="subtitle"
  >
    <b-row>
      <b-col cols="12" lg="3">
        <b-btn
          variant="success"
          @click="newGame()"
          placeholder="Testing"
          v-b-tooltip.hover
          title="Press this button to start a new game!"
        >
          New game
        </b-btn>
        <p v-if="gameId" class="small">
Please enter movements from both sides. For example:<br/>
e2e4<br/>
e7e6<br/>
...<br/>
Each row represents the movement of each side, starting with the white pieces.
        </p>
        <b-textarea
          class="mt-2"
          rows="15"
          v-model="moves"
          v-if="gameId"
          :disabled="movesUploaded"
        />
        <b-btn
          v-if="gameId && moves && !movesUploaded && !showEnd"
          variant="primary"
          @click="uploadMoves()"
          v-b-tooltip.hover
          title="Press this button to upload moves to this game"
        >
          Upload moves
        </b-btn>
        <b-btn
          v-if="movesUploaded"
          variant="warning"
          @click="nextMove()"
          v-b-tooltip.hover
          title="Read next move!"
        >
          Next move!
        </b-btn>
      </b-col>
      <b-col cols="12" lg="6">
        <img src="@/assets/background/welcome.png"  style="width:100%;border-radius:10px"  v-if="!board"/>
        <game-board :board="board" v-if="board" />
      </b-col>
      <b-col cols="12" lg="3">
        <b-card
          style="background-color: rgba(255, 255, 255, 0.8) !important"
          title="Messages and warnings!"
        >
          <b-alert v-if="showEnd" v-bind:show="showEnd" variant="success">
            No more moves to read. Game over!
          </b-alert>
          <b-alert
            v-if="moveInformation"
            v-bind:show="moveInformation.error"
            variant="danger"
          >
            {{ moveInformation.errorMessage }}
          </b-alert>
          <b-alert
            v-if="moveInformation"
            v-bind:show="moveInformation.pieceInCheck != null"
            variant="danger"
          >
            WARNING CHECK!!!!
          </b-alert>
        </b-card>
      </b-col>
    </b-row>
  </b-card>
</template>
<script>
import axios from "@/axios";
import GameBoard from "./components/GameBoard.vue";

export default {
  components: {
    GameBoard,
  },
  data: () => ({
    showEnd: false,
    gameId: null,
    players: null,
    board: null,
    moves: null,
    movesUploaded: false,
    moveInformation: null,
    subtitle: null,
  }),
  methods: {
    nextMove() {
      axios
        .get(
          "chess/nextMove/" +
            this.gameId
        )
        .then((response) => {
          this.moveInformation = response.data;
          if (!this.moveInformation.error) {
            if (this.moveInformation.lastMove) {
              this.showEnd = true;
              this.movesUploaded = false;
            }
            this.getBoard();
          }
        });
    },
    uploadMoves() {
      axios
        .post(
          "chess/uploadMoves/" +
            this.gameId,
          this.moves
        )
        .then((response) => {
          if (response.status == 200) {
            this.movesUploaded = true;
          } else {
            this.movesUploaded = false;
          }
        });
    },
    newGame() {
      this.showEnd = false;
      this.movesUploaded = false;
      axios
        .get(
          "chess/newGame"
        )
        .then((response) => {
          this.gameId = response.data.gameId;
          this.subtitle = "Game number: " + this.gameId;
          this.getBoard();
        });
    },
    getBoard() {
      axios
        .get(
          "chess/board/" +
            this.gameId
        )
        .then((response) => {
          this.board = response.data;
        });
    },
  },
};
</script>
<style>
.BLACK {
  background-color: #d18b47;
  width: 70px;
  height: 70px;
  font-size: 40px;
}

.WHITE {
  background-color: #ffce9e;
  min-height: 30px;
  width: 70px;
  height: 70px;
  font-size: 40px;
}

.reference {
  width: 30px;
  height: 30px;
}

.letter_reference {
  width: 70px;
  height: 30px;
}
.body {
  background-image: url("assets/background/chess.jpg") !important;
  background-size: cover !important;
}
</style>