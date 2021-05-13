<template>
  <article class="cart-item">
    <img class="cart-item__image" :src="imageURL" alt="" />
    <div class="cart-item__info">
      <div class="cart-item__info-row">
        <p class="cart-item__name">{{ name }}</p>
        <button
          class="cart-item__btn-close"
          aria-label="Remove product"
          @click="removeProduct"
        >
          <base-icon name="x" :size="18" />
        </button>
      </div>
      <div class="cart-item__info-row">
        <div class="cart-item__quantity">
          <button class="cart-item__btn-minus" @click="incrementQuantity">
            <base-icon name="minus" :size="15" />
          </button>
          <p>{{ quantity }} {{ unit }}</p>
          <button class="cart-item__btn-plus" @click="decrementQuantity">
            <base-icon name="plus" :size="15" />
          </button>
        </div>
        <div class="cart-item__price">${{ formatPrice(totalPrice) }}</div>
      </div>
    </div>
  </article>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue'
import { getSalePrice, getRegularPrice, getCurrentPrice } from '@/utils'
import { ProductPrice } from '@/types'

export default defineComponent({
  name: 'ShoppingCartItem',
  props: {
    id: {
      type: Number,
      required: true,
    },
    imageURL: {
      type: String,
      required: true,
    },
    name: {
      type: String,
      required: true,
    },
    price: {
      type: Array as PropType<ProductPrice[]>,
      required: true,
    },
    unit: {
      type: String,
      required: true,
    },
    rate: {
      type: Number,
      required: true,
    },
    quantity: {
      type: Number,
      required: true,
    },
  },
  computed: {
    salePrice (): number | undefined {
      return getSalePrice(this.price)
    },
    regularPrice (): number | undefined {
      return getRegularPrice(this.price)
    },
    currentPrice (): number | undefined {
      return getCurrentPrice(this.price)
    },
    totalPrice (): number | undefined {
      if (this.currentPrice) {
        return this.currentPrice * this.quantity
      }
      return undefined
    },
  },
  methods: {
    formatPrice (price: number): string {
      return price.toFixed(2)
    },
    removeProduct () {
      this.$store.dispatch('removeProductFromCart', this.id)
    },
    decrementQuantity () {
      this.$store.dispatch('incrementProductQuantity', this.id)
    },
    incrementQuantity () {
      this.$store.dispatch('decrementProductQuantity', this.id)
    },
  },
})
</script>

<style lang="scss" scoped>
@use '@/assets/css/vars' as vars;

.cart-item {
  display: flex;
  padding: 12px 0;
  border-bottom: 1px solid vars.$dark-cream;

  &__image {
    height: 44px;
  }

  &__info {
    margin-left: 16px;
    display: flex;
    flex-direction: column;
    flex-grow: 1;
  }

  &__info-row {
    align-items: center;
    display: flex;
    justify-content: space-between;

    &:first-of-type {
      margin-bottom: 12px;
      align-items: start;
    }
  }

  &__name {
    text-align: left;
    font-weight: bold;
  }

  &__btn-close {
    background-color: transparent;
    cursor: pointer;
  }

  &__quantity {
    display: flex;
    border: 1px solid vars.$light-cream;
    border-radius: vars.$border-radius;
    align-items: center;
  }

  &__btn-minus,
  &__btn-plus {
    padding: 6px;
    background-color: transparent;
    cursor: pointer;
  }
}
</style>