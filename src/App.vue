<template>
  <v-app>
    <v-content>
      <v-container>
        <v-row align="start" justify="center">
          <v-col cols="12">
            <v-img
              position="center center"
              src="https://penguin.upyun.galvincdn.com/logos/penguin_stats_logo.png"
              aspect-ratio="1"
              height="128px"
              contain
              class="my-4"
            />

            <h2 class="overline font-weight-bold text-center">
              企鹅物流数据统计
            </h2>
            <h1 class="heading text-center">
              镜像站选择
            </h1>

            <v-alert border="left" type="info" color="blue lighten-2" class="my-3" outlined>
              所有镜像站内的内容是一样的：不同服务器的数据内容，和所进入的镜像站是什么无关
            </v-alert>
          </v-col>

          <v-col
            v-for="(mirror, i) in mirrors"
            :key="i"
            cols="12"
            sm="6"
          >
            <v-card hover @click="go(mirror.url)">
              <v-img
                class="white--text align-center text-center"
                :height="picHeight"
                :src="`https://penguin.upyun.galvincdn.com/backgrounds/${mirror.id}.jpg`"
                gradient="to top right, rgba(100,115,201,.33), rgba(25,32,72,.7)"
              >
                <span
                  style="font-size: 64px"
                  class="white--text monospace"
                >
                  {{ mirror.icon.tld }}
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

    <v-footer color="transparent"  class="white--text text-center">
      <v-card-text class="white--text">
        <strong>Penguin Statistics</strong>
        —
        <strong>{{ year }}</strong>
      </v-card-text>
    </v-footer>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    mirrors: [
      {
        id: "cn",
        icon: {
          tld: ".cn"
        },
        url: "https://penguin-stats.cn",
        new: true
      },
      {
        id: "global",
        icon: {
          tld: ".io"
        },
        url: "https://penguin-stats.io",
      }
    ]
  }),

  methods: {
    go (url) {
      document.location = url;
    }
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
    letter-spacing: -.15rem;
    text-shadow: 0 0 10px rgba(0, 0, 0, .5);
  }
</style>
