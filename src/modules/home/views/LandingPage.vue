<template>
  <div>
    <h1>{{ $t('home.LandingPage.title') }}</h1>
    <v-row>
      <v-col class="container">
        <ManagerState />
        <SlamActions />
      </v-col>
      <v-col class="container">
        <MapSelect />
        <RelocInit />
        <ProfileSelect />
      </v-col>
    </v-row>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import ManagerState from '@/modules/home/components/ManagerState.vue';
import SlamActions from '@/modules/home/components/SlamActions.vue';
import MapSelect from '@/modules/home/components/MapSelect.vue';
import RelocInit from '@/modules/home/components/RelocInit.vue';
import ProfileSelect from '@/modules/home/components/ProfileSelect.vue';

@Component({
  components: { ManagerState, SlamActions, MapSelect, RelocInit, ProfileSelect }
})
export default class LandingPage extends Vue {}
</script>

<style scoped lang="scss">
.container > :not(:first-child) {
  margin-top: 10px;
}
</style>
