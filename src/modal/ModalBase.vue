<template lang="pug">
Transition(name="disapear")
  div(v-if="isVisible", :class="$style.cont")
    div(:class="$style.head")
      font-awesome-icon(
        :class="$style.close"
        :icon="['fas', 'circle-xmark']"
        @click="handleClose"
      )
    div(:class="$style.body") 
      slot
    div(:class="$style.bottom")
      button(:class="[$style.btn, $style.izq, 'a']", @click="handleLeft") {{labelIzq}}
      button(:class="[$style.btn, $style.der, 'b']", @click="handleRight") {{labelDer}}

Transition(name="disapear")
  div(v-if="isVisible", :class="$style.back")
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  emit: ["close", "left", "right"],
  props: {
    isVisible: Boolean,
    labelIzq: {
      type: String,
      required: true,
      default: "Cancel",
    },
    labelDer: {
      type: String,
      required: true,
      default: "Accept",
    },
  },
  setup(props, { emit }) {
    const handleClose = () => emit("close");
    const handleLeft = () => emit("left");
    const handleRight = () => emit("right");
    return {
      handleClose,
      handleLeft,
      handleRight,
    };
  },
});
</script>

<style module lang="stylus">
@import "../assets/base"
.cont
  box-shadow: 5px 10px 8px color-dark-shadow;
  box-sizing border-box
  border-radius br-md
  background-color color-dark
  display flex
  flex-direction column
  width 310px
  height 512px
  position fixed
  top pd-lg
  left calc(50% - 310px/2)
  z-index 999
  .head
    box-sizing border-box
    border-radius br-md br-md 0 0
    height 46px
    background-color color-main
    display flex
    justify-content flex-end
    color color-light
    .close
      height icon-sm
      width icon-sm
      padding pd-sm
      &:hover
        cursor pointer
  .body
    box-sizing border-box
    height 100%
  .bottom
    box-sizing border-box
    height 46px
    display flex
    justify-content space-between
    .btn
      height 46px
      justify-content center
      align-items center
      color color-light
      width 100%
      &:hover
        color color-light
        cursor pointer
    .izq
      border-radius 0 0 0 br-md
    .der
      border-radius 0 0 br-md 0
.back
  position fixed
  min-width 100%
  min-height 100%
  background-color color-dark-trans
  top 0
  left 0
  z-index 998
</style>
