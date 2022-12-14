<template>
  <v-card>
    <v-card-title>{{ $t('home.SlamActions.cardTitle') }}</v-card-title>
    <v-card-text>
      <v-btn
        width="150"
        v-show="!isActive()"
        :loading="waitingForManagerState === 'SLAM' || waitingForManagerState === 'LOCALIZING'"
        :disabled="!isReadyToStart()"
        @click="startSlam"
        color="primary"
      >
        {{ $t('home.SlamActions.startSlam') }}
      </v-btn>
      <v-btn
        width="150"
        v-show="isActive()"
        :loading="waitingForManagerState === 'IDLE' || waitingForManagerState === 'RELOC_READY'"
        @click="stopSlam"
        color="accent"
      >
        {{ $t('home.SlamActions.stopSlam') }}
      </v-btn>
    </v-card-text>
  </v-card>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import { State, Mutation, Action } from 'vuex-class';
import * as CommandsService from '@/modules/home/services/commands';

@Component({
  components: {}
})
export default class SlamActions extends Vue {
  @State((state) => state.home.currentManagerState)
  protected currentManagerState?: string;
  @State((state) => state.home.waitingForManagerState)
  protected waitingForManagerState?: string | null;
  @State((state) => state.home.selectedMap)
  protected selectedMap?: string;
  @Mutation('home/setSelectedMap')
  protected setSelectedMap!: (value: string | null) => void;
  @Action('home/waitForState')
  protected waitForState!: (waitingState: string) => Promise<void>;

  protected startSlam(): void {
    if (this.selectedMap) {
      CommandsService.startReloc();
      this.waitForState('LOCALIZING');
    } else {
      CommandsService.startSlam();
      this.waitForState('SLAM');
    }
  }

  protected stopSlam(): void {
    if (this.selectedMap) {
      CommandsService.stopReloc();
      this.waitForState('IDLE');
      this.setSelectedMap(null);
    } else {
      CommandsService.stopSlam();
      this.waitForState('IDLE');
    }
  }

  protected isActive(): boolean {
    return this.currentManagerState === 'LOCALIZING' || this.currentManagerState === 'SLAM';
  }

  protected isReadyToStart(): boolean {
    return (
      (this.currentManagerState === 'IDLE' || this.currentManagerState === 'RELOC_READY') &&
      this.waitingForManagerState === null
    );
  }
}
</script>

<style scoped lang="scss"></style>
