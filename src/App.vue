<template>
  <v-app id="inspire">
    <v-toolbar color="#2c3e50" dark fixed app>
      <!-- <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon> -->
      <v-toolbar-title color="#58b7ff">臺南好好玩</v-toolbar-title>
      <div class="hidden-sm-and-down">
        <v-btn flat value="attractions" to="attractions">
          <v-icon class="mr-1">fas fa-map</v-icon>
          <span>景點</span>
        </v-btn>

        <v-btn flat value="weather" to="weather">
          <v-icon class="mr-1">fas fa-sun</v-icon>
          <span>氣象資訊</span>
        </v-btn>

        <v-btn flat value="wishlist" to="wishlist">
          <v-icon class="mr-1">fas fa-heart</v-icon>
          <span>願望清單</span>
          <v-badge
            v-if="wishlistBadgeVal > 0"
            color="red"
            style="position:absolute; right:-5%; top:-5%;">
            <template v-slot:badge>
              <span>{{ wishlistBadgeVal }}</span>
            </template>
          </v-badge>
        </v-btn>
      </div>
      <v-spacer></v-spacer>
    </v-toolbar>
    <v-content>
      <v-container fluid>
        <router-view
          :val="wishlistBadgeVal"
          @wishlistValChanged="wishlistBadgeVal = $event">
        </router-view>
        <v-btn
            v-scroll="onScroll"
            v-show="fab"
            fab
            dark
            fixed
            bottom
            right
            color="primary"
            @click="toTop"
          >
            <v-icon>fas fa-angle-up</v-icon>
          </v-btn>
      </v-container>
    </v-content>
    <v-card height="56px" flat class="hidden-md-and-up">
      <v-bottom-nav :active.sync="bottomNav" :value="true" fixed>
        <v-btn color="teal" flat value="attractions" to="attractions">
          <span>景點</span>
          <v-icon>fas fa-map</v-icon>
        </v-btn>

        <v-btn color="teal" flat value="weather" to="weather">
          <span>氣象資訊</span>
          <v-icon>fas fa-sun</v-icon>
        </v-btn>

        <v-btn color="teal" flat value="wishlist" to="wishlist">
          <span>願望清單</span>
          <v-icon>fas fa-heart</v-icon>
          <v-badge
            v-if="wishlistBadgeVal > 0"
            color="red"
            style="position:absolute; right:30%; top:15%;">
            <template v-slot:badge>
              <span>{{ wishlistBadgeVal }}</span>
            </template>
          </v-badge>
        </v-btn>
      </v-bottom-nav>
    </v-card>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    drawer: null,
    wishlistBadgeVal: 0,
    bottomNav: 'attractions',
    fab: false,
  }),
  props: {
    source: String,
  },
  computed: {
    isWishlisthidden() {
      return !(this.wishlistBadgeVal > 0);
    },
  },
  methods: {
    onScroll(e) {
      if (typeof window === 'undefined') return;
      const top = window.pageYOffset ||   e.target.scrollTop || 0;
      this.fab = top > 20;
    },
    toTop() {
      this.$vuetify.goTo(0);
    },
  },
  mounted() {
    const wl = localStorage.getItem('wishlist');
    this.wishlistBadgeVal = wl == null ? 0 : JSON.parse(wl).length;
  },
};
</script>

<style>
#app {
  font-family: "Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB",
    "Microsoft YaHei", "微软雅黑", Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

#logo {
  font-size: 24px;
  color: #13ce66 !important;
}
</style>
