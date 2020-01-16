<template>
  <v-app id="inspire">
    <!-- <v-navigation-drawer
      v-model="drawer"
      app
      right
    >
      <v-list dense>
        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-home</v-icon>
          </v-list-item-action>

          <v-list-item-content>
            <v-list-item-title>Home</v-list-item-title>
          </v-list-item-content>
        </v-list-item>

        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-contact-mail</v-icon>
          </v-list-item-action>

          <v-list-item-content>
            <v-list-item-title>Contact</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer> -->

    <v-app-bar
      app
      color="cyan"
    >

      <v-toolbar-title style="color: white;">Rakugaki2Model</v-toolbar-title>

      <v-spacer />
      <!-- <v-app-bar-nav-icon @click.stop="drawer = !drawer" /> -->
      <!-- <v-overflow-btn
          :items="['a', 'b']"
          label="Segmented Btn"
          target="#dropdown-example"
        ></v-overflow-btn> -->
    </v-app-bar>

    <v-content>
      <nuxt />
    </v-content>

    <v-footer
      color="cyan"
      app
    >
      <!-- <v-spacer /> -->

      <span class="white--text">&copy; HAL NAGOYA H430 Ogawa-Gumi</span>
    </v-footer>
  </v-app>
</template>

<script>
  export default {
    name: 'LayoutsDemosBaselineFlipped',

    props: {
      source: String,
    },
    data: () => ({
      drawer: null,
    }),
  }
</script>
