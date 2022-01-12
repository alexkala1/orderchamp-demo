<template>
  <v-row>
    <v-col>
      <v-card flat>
        <!-- header of cart -->
        <v-card-subtitle>
          {{ productName }} - €100 Order minimum
        </v-card-subtitle>

        <v-card-title>
          <v-row align="start">
            <v-col cols="auto">
              <v-rating
                v-model="rating"
                background-color="yellow"
                color="yellow accent-4"
                dense
                half-increments
                hover
              >
              </v-rating>
            </v-col>
            <v-col cols="auto">
              {{ rating.toFixed(1) }}
            </v-col>
          </v-row>
        </v-card-title>

        <v-card-title> {{ translatedTitle }} </v-card-title>
        <v-card-subtitle class="orange--text">
          {{ productDiscount }}% discount
        </v-card-subtitle>
        <!-- end header of cart -->

        <!-- Cart table -->
        <v-card-text>
          <v-simple-table>
            <template #default>
              <thead>
                <tr>
                  <th
                    v-for="header in productHeaders"
                    :key="header"
                    class="text-left"
                  >
                    {{ header }}
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="variant in productVariants" :key="variant.name">
                  <td style="width: 140px">
                    <v-text-field
                      v-model.number="variant.quantity"
                      :value="variant.quantity"
                      single-line
                      outlined
                      hide-details
                      append-outer-icon="mdi-plus"
                      prepend-icon="mdi-minus"
                      @click:append-outer="variant.quantity++"
                      @click:prepend="
                        variant.quantity > 0 ? variant.quantity-- : null
                      "
                    ></v-text-field>
                  </td>
                  <td class="font-weight-bold">{{ variant.variant }}</td>
                  <td>
                    <s> €{{ variant.price }} </s>
                    <span class="red--text">
                      €{{ calculateDiscount(variant.price, productDiscount) }}
                    </span>
                  </td>
                  <td>
                    €{{ variant.msrp }}
                    <span class="green--text"
                      >x{{
                        calculatePriceDrop(
                          variant.msrp,
                          variant.price,
                          productDiscount
                        )
                      }}
                    </span>
                  </td>
                  <td v-if="variant.inStock === true">
                    <v-icon color="green">mdi-check</v-icon>
                  </td>
                  <td v-else>
                    <v-icon color="red">mdi-cross</v-icon>
                  </td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-card-text>
        <!-- end of cart table -->

        <!-- Cart actions -->
        <v-card-actions>
          <v-row>
            <v-col cols="8">
              <v-col cols="12" class="green--text">
                <v-icon color="green">mdi-check</v-icon> Available
                <br />
                Ships directly from brand - 1-3 days
              </v-col>
              <v-col cols="12">
                <v-btn x-large block color="green" dark>
                  <v-row dense justify="space-between">
                    <v-col> add to cart </v-col>
                    <v-col> €{{ finalPrice }} </v-col>
                  </v-row>
                </v-btn>
              </v-col>
              <v-col cols="12">
                <v-btn block color="black" dark>
                  <v-icon>mdi-heart</v-icon>
                  Add to favorites
                </v-btn>
              </v-col>
              <v-col cols="12">
                <v-btn block outlined>Contact the brand</v-btn>
              </v-col>
              <v-col cols="12">
                <v-card color="grey lighten-2">
                  <v-card-text>
                    <v-row>
                      <v-col cols="auto">
                        <v-avatar color="white">
                          <v-icon>mdi-shield-half-full </v-icon>
                        </v-avatar>
                      </v-col>
                      <v-col class="black--text">
                        Learn about
                        <a class="black--text" href="">
                          Buyer protection and secured payments
                        </a>
                      </v-col>
                    </v-row>
                  </v-card-text>
                </v-card>
              </v-col>
            </v-col>
          </v-row>
        </v-card-actions>
        <!-- End cart actions -->
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  props: [
    'productName',
    'productRating',
    'productTitle',
    'productDiscount',
    'translatedTitle',
    'productHeaders',
    'productVariants',
  ],

  data() {
    return {
      rating: this.productRating,
    }
  },

  computed: {
    /**
     * A computed property that calculates the cart price due to
     * having a complex logic.
     */
    finalPrice() {
      const allprices = this.productVariants.map(
        (variant) =>
          variant.quantity *
          this.calculateDiscount(variant.price, this.productDiscount)
      )

      return allprices.reduce((a, b) => a + b, 0).toFixed(2)
    },
  },

  methods: {
    /**
     * A method that calculates the final price with its discount
     */
    calculateDiscount(price, discount) {
      const discountAmount = price * (discount / 100)
      const finalPrice = price - discountAmount

      return finalPrice.toFixed(2)
    },

    /**
     * A method that returns how many times up is the MSRP price from
     * the price with the discount.
     */
    calculatePriceDrop(msrp, price, discount) {
      const discountPrice = this.calculateDiscount(price, discount)

      return (msrp / discountPrice).toFixed(1)
    },
  },
}
</script>

<style></style>
