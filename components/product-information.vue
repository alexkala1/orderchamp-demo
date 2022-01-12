<template>
  <v-row justify="center">
    <!-- Carousel component with image picker -->
    <v-col cols="12">
      <v-carousel v-model="model" hide-delimiters>
        <v-carousel-item v-for="(image, index) in images" :key="index">
          <v-img :src="images[model].carousel"> </v-img>
        </v-carousel-item>
      </v-carousel>
      <v-slide-group
        v-model="model"
        class="pa-4"
        center-active
        show-arrows="always"
      >
        <v-slide-item
          v-for="(image, index) in images"
          :key="index"
          v-slot="{ active, toggle }"
        >
          <v-card class="ma-4" height="70" width="70" @click="toggle">
            <v-img
              :src="image.preview"
              height="70"
              width="70"
              :style="active ? 'border: 1px solid black' : ''"
            >
            </v-img>
          </v-card>
        </v-slide-item>
      </v-slide-group>
    </v-col>
    <!-- End Carousel component with image picker -->

    <!-- Expansion panels component -->
    <v-col cols="12">
      <v-divider></v-divider>
      <v-card flat>
        <v-card-text>
          <v-expansion-panels v-model="panels" accordion multiple flat>
            <v-expansion-panel
              v-for="(panel, index) in productPanels"
              :key="index"
            >
              <v-expansion-panel-header class="title">
                {{ panel }}
              </v-expansion-panel-header>
              <v-expansion-panel-content>
                <span style="white-space: pre-line">
                  {{ productDescription }}
                </span>
                <h5
                  class="pt-2 float-right font-weight-normal font-italic grey--text"
                >
                  Machine Translation
                  <span class="translate-text" @click="translateEvent">
                    {{ translationText }}
                  </span>
                </h5>
              </v-expansion-panel-content>
              <v-divider></v-divider>
            </v-expansion-panel>
          </v-expansion-panels>
        </v-card-text>
      </v-card>
    </v-col>
    <!-- End of expansion panels component -->
  </v-row>
</template>

<script>
export default {
  props: ['productDescription', 'images', 'translationText', 'productPanels'],
  data() {
    return {
      model: 0,
      panels: [0],
    }
  },

  methods: {
    /**
     * We emit an event from this component to its parent
     * to communicate the button press in order to translate
     * the information text to the appropriate language.
     */
    translateEvent() {
      this.$emit('translate')
    },
  },
}
</script>

<style>
.translate-text {
  text-decoration: underline;
  cursor: pointer;
  color: black;
}
</style>
