<template>
  <div class="popup" v-if="show" @click.stop="hidePopup">
    <div @click.stop class="popup__content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import toggleMixin from "@/mixins/toggleMixin";

export default {
  name: 'my-popup',
  mixins: [toggleMixin]
}
</script>

<style scoped>
.popup {
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background: rgba(0,0,0,.5);
  position: fixed;
  display: flex;
}
.popup__content {
  margin: auto;
  background: #fff;
  border-radius: .5rem;
  min-height: 100px;
  min-width: 400px;
  padding: 1.5rem;
}
</style>