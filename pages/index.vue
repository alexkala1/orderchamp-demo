<template>
  <v-container fluid>
    <v-row>
      <v-col cols="12" lg="7">
        <product-information
          :product-description="productDescription"
          :translation-text="productTranslationText"
          :images="productImages"
          :product-panels="productPanels"
          @translate="translateText"
        ></product-information>
      </v-col>
      <v-col cols="12" lg="5">
        <product-cart
          :product-rating="5"
          :product-discount="20"
          :product-headers="productHeaders"
          :product-name="productName"
          :translated-title="productTitle"
          :product-variants="productVariants"
        ></product-cart>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      productDescription: null,
      translatedProductDescription: null,
      productTranslationText: 'See translated',
      productEnglishTranslationText: 'See original language',
      productHeaders: ['Quantity', 'Variant', 'Price', 'MSRP', 'Stock'],
      apiData: null,
      productName: null,
      productTitle: null,
      productVariants: [],
      productImages: [],
      productPanels: ['Information', 'Product Specifications', 'Reviews'],
    }
  },

  mounted() {
    this.fetchData()
  },

  methods: {
    /**
     * This method is used when the event from product-information child component
     * is triggered. It switches the values from both the texts that need to be translated
     * since we don't use a i18n solution.
     */
    translateText() {
      // description text
      ;[this.productDescription, this.translatedProductDescription] = [
        this.translatedProductDescription,
        this.productDescription,
      ]

      // translation link text
      ;[this.productTranslationText, this.productEnglishTranslationText] = [
        this.productEnglishTranslationText,
        this.productTranslationText,
      ]
    },

    /**
     * We have a main method to fetch data from the external API.
     * We then proceed to distribute the data to its appropriate variable
     * so that we can pass the values to the child components
     */
    async fetchData() {
      this.apiData = await this.$axios.get(
        'https://gist.githubusercontent.com/Vugario/ca7cc468c344494ccae22b2c835e0c9f/raw/fe4669ee0989e78be1ed9ccd78ba178c695c305a/data.json'
      )

      this.productName = this.apiData.data.data.storefrontBySlug.name

      this.productDescription =
        this.apiData.data.data.storefrontBySlug.listing.description

      this.translatedProductDescription =
        this.apiData.data.data.storefrontBySlug.listing.translation.description

      this.productTitle = this.apiData.data.data.storefrontBySlug.listing.title

      this.apiData.data.data.storefrontBySlug.listing.variants.edges.forEach(
        (variant) => {
          this.productVariants.push({
            quantity: 0,
            variant: variant.node.options[0].value,
            price: variant.node.price,
            msrp: variant.node.msrp,
            inStock: true,
          })
        }
      )

      this.apiData.data.data.storefrontBySlug.listing.images.edges.forEach(
        (image) => {
          this.productImages.push({
            preview: image.node.modal,
            carousel: image.node.small,
          })
        }
      )
    },
  },
}
</script>
