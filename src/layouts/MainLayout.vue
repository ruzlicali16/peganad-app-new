<template>
  <q-layout view="lHh lpr lFf">
    <q-header :class="headerColor()">
      <q-toolbar
        v-if="['/learn', `/learn/${$route.params.id}`].find((p) => p == $route.path)"
      >
        <q-btn
          flat
          round
          :icon="$route.name == 'learn-content' ? 'close' : 'arrow_back'"
          :to="$route.path == '/learn' ? '/' : '/learn'"
        />
        <q-toolbar-title
          v-if="$route.path == `/learn/${$route.params.id}`"
          class="text-h5 text-weight-bold text-uppercase absolute-center"
          >Learn {{ $route.params.id }}</q-toolbar-title
        >
      </q-toolbar>
      <q-toolbar
        v-else-if="['/game', `/game/${$route.params.id}`].find((p) => p == $route.path)"
      >
        <q-btn
          flat
          round
          :icon="$route.name == 'game-content' ? 'close' : 'arrow_back'"
          :to="$route.path == '/game' ? '/' : '/game'"
          @click="closeGame()"
        />
        <q-toolbar-title
          v-if="$route.path == `/game/${$route.params.id}`"
          class="text-h5 text-weight-bold text-uppercase absolute-center"
          >Play {{ $route.params.id }}</q-toolbar-title
        >
        <q-space></q-space>
        <q-btn flat round to="/board" v-if="$route.path == '/game'" icon="emoji_events" />
      </q-toolbar>
    </q-header>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script lang="ts">
import {Vue, Component} from 'vue-property-decorator';

@Component({})
export default class MainLayout extends Vue {
  headerColor() {
    if (this.onLearnPage()) {
      return 'bg-pink-2';
    } else if (this.$route.path == `/learn/${this.$route.params.id}`) {
      return 'bg-info';
    } else if (this.onGamePage()) {
      return 'bg-green-3';
    } else if (this.$route.path == `/game/${this.$route.params.id}`) {
      return 'bg-info';
    } else {
      return 'bg-white';
    }
  }

  onLearnPage() {
    return this.$route.path == '/learn';
  }

  onGamePage() {
    return this.$route.path == '/game';
  }

  closeGame() {}
}
</script>
