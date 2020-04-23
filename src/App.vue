<template>
  <v-app>
    <v-content class="transparent">
      <Background />
      <v-container class="transparent">
        <v-row align="start" justify="center">
          <div class="locale-switcher">
            <LocaleSwitcher />
          </div>

          <v-col cols="12">
            <v-img
              position="center center"
              src="https://penguin.upyun.galvincdn.com/logos/penguin_stats_logo.png"
              aspect-ratio="1"
              height="128px"
              contain
              class="my-4"
            />

            <h2 class="overline font-weight-bold text-center z-index-fix">
              {{ $t('app.name') }}
            </h2>
            <h1 class="heading text-center z-index-fix">
              {{ $t('app.title') }}
            </h1>

            <v-alert border="left" type="info" color="blue lighten-2" class="mt-4 mb-2" outlined>
              {{ $t('app.notice') }}
            </v-alert>
          </v-col>

          <v-col
            v-for="(mirror, i) in mirrors"
            :key="i"
            cols="12"
            sm="6"
          >
            <v-card hover :href="mirror.url" class="card-background">
              <v-img
                class="white--text align-center text-center"
                :height="picHeight"
                :src="`https://penguin.upyun.galvincdn.com/backgrounds/${mirror.id}.jpg`"
                gradient="to top right, rgba(100,115,201,.33), rgba(25,32,72,.7)"
              >
                <span
                  style="font-size: 48px"
                  class="white--text monospace"
                >
                  <span class="normal">{{mirror.icon.domain}}</span><span class="emphasize">.{{ mirror.icon.tld }}</span>
                </span>
              </v-img>
              <v-divider/>
              <v-card-title
                class="darken-1"
              >
                {{ $t(['mirrors', mirror.id, 'title'].join('.')) }}
                <v-icon v-if="mirror.new" right>
                  mdi-alert-decagram
                </v-icon>
              </v-card-title>
              <v-card-subtitle
                class="darken-1"
              >
                {{ $t(['mirrors', mirror.id, 'subtitle'].join('.')) }}
              </v-card-subtitle>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>

    <v-footer color="transparent" class="white--text text-center">
      <v-card-text class="white--text">
        <strong>Penguin Statistics</strong>
        —
        <strong>{{ year }}</strong>
      </v-card-text>
    </v-footer>
  </v-app>
</template>

<script>

import strings from "./utils/strings";
import I18n from "./mixins/I18n";
import LocaleSwitcher from "./components/LocaleSwitcher";
import Background from "./components/Background";

export default {
  name: 'App',
  components: {Background, LocaleSwitcher},
  mixins: [I18n],

  data: () => ({
    mirrors: [
      {
        id: "cn",
        icon: {
          domain: "penguin-stats",
          tld: "cn"
        },
        url: "https://penguin-stats.cn/?utm_source=choose-mirror&utm_medium=card-option",
        new: true
      },
      {
        id: "global",
        icon: {
          domain: "penguin-stats",
          tld: "io"
        },
        url: "https://penguin-stats.io/?utm_source=choose-mirror&utm_medium=card-option",
      }
    ]
  }),

  created () {
    const language = strings.getFirstBrowserLanguage();
    console.info("i18n", "detected language", language);
    if (language) this.changeLocale(language)
  },

  computed: {
    year() {
      return new Date().getFullYear();
    },
    picHeight () {
      return this.$vuetify.breakpoint.xs ? "128px" : "256px"
    }
  },
}
</script>

<style>
  .monospace {
    font-family: "Roboto Mono", "Fira Code", "Roboto", sans-serif;
    font-weight: 500;
  }
  .monospace .normal {
    font-size: 20px
  }
  .monospace .emphasize{
    letter-spacing: -.15rem;
    text-shadow: 0 0 10px rgba(0, 0, 0, .5);
  }
  .monospace .wide {
    letter-spacing: 0 !important;
  }

  .locale-switcher {
    position: fixed !important;
    right: 1em !important;
    top: 1em !important;
    z-index: 99999999;
  }

  .z-index-fix {
    z-index: 5 !important;
  }

  .v-application, .v-application--wrap {
    background: transparent !important;
  }

  .card-background {
    background: rgba(20, 20, 20, .8) !important;
  }

  .theme--dark.v-sheet.v-alert {
    background: rgba(0, 0, 0, .7) !important;
  }
</style>
