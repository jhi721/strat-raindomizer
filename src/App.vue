<template>
  <div id="app">
    <button
      class="strat-button"
      v-if="status === 'Waiting'"
      @click="randomizeStrat"
    >
      Get Pro Strat
    </button>

    <Strat v-else :strat="currentStrat" :status="status" />

    <RefreshButton @click.native="resetState" :status="status" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Strat from '@/components/Strat.vue';
import RefreshButton from '@/components/RefreshButton.vue';

@Component({
  components: {
    RefreshButton,
    Strat,
  },
})
export default class App extends Vue {
  private strats: string[] = [
    'Pistols Only',
    'SMG Only',
    'DMR Only',
    'Shotgun Only',
    'Shields Only',
    'LMG Only',
    'Hipfire Only',
    'Dropshot Only',
    'Prone(Def Only)',
    'Crouch Only',
    'Burst Mode',
    'One-Tap Mode',
    'Sniper Rifle',
    'One-Bullet Mag',
    'Teamkill',
    'Suicide',
  ];

  private currentStrat = '';
  private status: 'Waiting' | 'Randomizing' | 'Completed' = 'Waiting';

  public randomizeStrat(): void {
    if (this.status !== 'Randomizing') {
      this.status = 'Randomizing';

      const interval = setInterval(() => {
        this.currentStrat = this.getRandomStrat();
      }, 150);

      setTimeout(() => {
        this.status = 'Completed';

        clearInterval(interval);
      }, 5000);
    }
  }

  public resetState(): void {
    this.currentStrat = '';
    this.status = 'Waiting';
  }

  public getRandomStrat(): string {
    const randomIndex = Math.floor(Math.random() * this.strats.length);

    return this.strats[randomIndex];
  }
}
</script>

<style lang="scss">
* {
  font-family: 'Recursive', sans-serif;
  color: #fff;
  box-sizing: border-box;
}

body {
  padding: 0;
  margin: 0;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
  background-image: url('assets/images/background.jpg');
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

.strat-button {
  position: relative;
  border: 2px solid #000;
  background: #56575c;
  font-size: 40px;
  overflow: hidden;
  color: inherit;
  cursor: pointer;
  padding: 10px 20px;
  transition: transform 0.2s;
  animation: appear 1s forwards;

  &:active {
    transform: scale(0.9) translateY(10px);
  }

  &::after,
  &::before {
    position: absolute;
    content: '';
    height: 50%;
    left: 0;
    right: 0;
    background: #000;
    mix-blend-mode: overlay;
    transition: transform 0.5s;
  }

  &::before {
    top: -50%;
  }

  &::after {
    top: 100%;
  }

  &:hover {
    &::before {
      transform: translateY(100%);
    }

    &::after {
      transform: translateY(-100%);
    }
  }
}
</style>
