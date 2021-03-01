<template>
  <img :src="url" style="width:100%; height:100%"/>
</template>
<script>
export default {
  props: {
    piece: {
      type: Object,
      required: true,
    },
  },
  computed: {
      url() {
          if (this.piece.side.white)
            return require('@/assets/pieces/white_'+this.piece.pieceType.toLowerCase()+'.png');
          else
            return require('@/assets/pieces/black_'+this.piece.pieceType.toLowerCase()+'.png');
      }
  }
};
</script>