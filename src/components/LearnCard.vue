<template>
  <div class="q-gutter-y-md" style="width: 100%">
    <q-card
      class="my-card bg-yellow-3 q-pt-xs q-ma-md"
      v-for="(content, index) in contents"
      :key="index"
    >
      <q-card-section class="text-center">
        <div class="text-h4">{{ content.name }}</div>
        <img :src="`data:image/jpeg;base64,${content.img}`" height="250" width="250" />
      </q-card-section>
      <q-card-actions class="column q-gutter-y-xs bg-white" align="center">
        <div class="text-h4">{{ content.translatedName }}</div>
        <q-btn
          class="full-width"
          size="lg"
          rounded
          push
          color="cyan-10"
          icon="volume_off"
          :loading="showAudioLoader && index == tappedIndex ? true : false"
          @click="playAudio(content.audio, index)"
        >
          <template v-slot:loading>
            <q-spinner-audio v-if="showAudioLoader" />
          </template>
        </q-btn>
      </q-card-actions>
    </q-card>
  </div>
</template>

<script lang="ts">
import {Vue, Component} from 'vue-property-decorator';
import {IContent} from 'src/interfaces/common-interface';
import helperService from 'src/services/helper.service';
import {mapState, mapActions} from 'vuex';

@Component({
  computed: {
    ...mapState('common', ['contents']),
  },
})
export default class LearnCard extends Vue {
  // Your local data for storing DB datas
  contents!: IContent[];
  showAudioLoader = false;
  tappedIndex = 0;

  async playAudio(base64string: string, index: number) {
    this.showAudioLoader = true;
    this.tappedIndex = index;
    let audioBase64 = 'data:audio/wav;base64,' + base64string;
    let audio: any = await helperService.playAudio(audioBase64);
    await audio.play();
    audio.onended = () => {
      this.showAudioLoader = false;
    };
  }
}
</script>

<style></style>
