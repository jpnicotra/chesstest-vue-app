<template>
  <div>
    <b-row v-for="(cell, index) of board.cells" v-bind:key="'letter_' + index">
      <template v-if="index == 0">
        <div
          class="d-inline-flex align-middle justify-content-center reference"
        ></div>
        <div
          v-for="(subcell, subIndex) of cell"
          v-bind:key="'letter_' + index + '_' + subIndex"
          class="d-inline-flex align-middle justify-content-center letter_reference"
        >
          {{ String.fromCharCode(97 + subIndex) }}
        </div>
      </template>
    </b-row>
    <b-row v-for="(cell, index) of board.cells" v-bind:key="index">
      <div class="d-inline-flex align-middle justify-content-center reference">
        {{ board.cells.length - index }}
      </div>
      <div
        v-for="(subcell, subIndex) of cell"
        v-bind:key="index + '_' + subIndex"
        class="d-inline-flex align-middle justify-content-center"
        :class="subcell.cellType"
      >
        <span v-if="subcell.piece">
          <chess-piece :piece="subcell.piece" />
        </span>
      </div>
    </b-row>
  </div>
</template>
<script>
import ChessPiece from "./ChessPiece.vue";

export default {
  components: {
    ChessPiece,
  },
  props: {
    board: {
      type: Object,
      required: true,
    },
  },
};
</script>
<style scoped>
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
</style>