<template>
  <img
    :class="[`reset-button`, { 'reset-button--show': status === 'Completed' }]"
    :src="require('@/assets/images/refresh-icon.svg')"
    alt="refresh icon"
  />
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component({
  name: 'RefreshButton',
})
export default class RefreshButton extends Vue {
  @Prop({ required: true }) readonly status!: string;
}
</script>

<style lang="scss" scoped>
.reset-button {
  position: absolute;
  width: 40px;
  height: 40px;
  top: -50px;
  border-radius: 100%;
  padding: 8px;
  cursor: pointer;
  background: #fff;
  transition: transform 0.5s ease-in-out;

  &--show {
    transform: translateY(65px) rotate(420deg);
  }
}

@keyframes appear {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
