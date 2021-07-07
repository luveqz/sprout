<template>
  <div class="content-max-width">
    <form class="container" @submit.prevent="">
      <p class="route-path">
        Homepage / Checkout
      </p>
      <h2 class="page-title">
        Checkout
      </h2>

      <div class="buyer-data">
        <fieldset class="buyer-data__fieldset">
          <legend class="buyer-data__legend">Personal information:</legend>
          <base-input-text class="buyer-data__input" label="First name" />
          <base-input-text class="buyer-data__input" label="Last name" />
          <base-input-text class="buyer-data__input" label="Phone" />
          <base-input-text class="buyer-data__input" label="Email" />
        </fieldset>

        <fieldset class="buyer-data__fieldset">
          <legend class="buyer-data__legend">Delivery details:</legend>
          <base-input-text class="buyer-data__input" label="Country / Region" />
          <base-input-text class="buyer-data__input" label="Town / City" />
          <base-input-text class="buyer-data__input" label="Street" />
          <base-input-text class="buyer-data__input" label="Postcode" />
          <base-input-text class="buyer-data__input" label="Packaging type" />
          <base-input-text class="buyer-data__input" label="Shipping option" />
        </fieldset>

        <fieldset class="buyer-data__fieldset">
          <legend class="buyer-data__legend">Payment:</legend>

          <div class="credit-card">
            <base-input-text
              class="buyer-data__input credit-card__number"
              label="Card number"
            />
            <base-input-text
              class="buyer-data__input credit-card__date"
              label="Expiration date"
            />
            <base-input-text
              class="buyer-data__input credit-card__cvv"
              label="CVV code"
            />
          </div>
        </fieldset>
      </div>

      <div class="order">
        <div class="order__summary">
          <div class="order__title">Your order:</div>
          <div class="order__row order__subtotal">
            <p>Subtotal:</p>
            <p>${{ formatPrice(shoppingCartSubTotal) }}</p>
          </div>
          <div class="order__row order__delivery">
            <p>Delivery:</p>
            <p>${{ formatPrice(deliveryPrice) }}</p>
          </div>
          <div class="order__row order__total">
            <p>Total:</p>
            <p>${{ formatPrice(shoppingCartTotal) }}</p>
          </div>
          <button class="btn btn--green btn--full">Purchase</button>
        </div>

        <div class="order__products">
          <shopping-cart-item
            v-for="item in shoppingCart"
            v-bind="item"
            :key="item.id"
          />
        </div>
      </div>
    </form>
  </div>
  <the-footer v-if="!loading" />
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { mapGetters, mapState } from 'vuex'
import TheFooter from '@/components/TheFooter.vue'
import ShoppingCartItem from '@/components/ShoppingCartItem.vue'

export default defineComponent({
  name: 'Home',
  components: {
    TheFooter,
    ShoppingCartItem,
  },
  computed: {
    ...mapState(['shoppingCart', 'deliveryPrice']),
    ...mapGetters(['shoppingCartSubTotal', 'shoppingCartTotal']),
  },
  methods: {
    formatPrice (price: number): string {
      return price.toFixed(2)
    },
  },
})
</script>

<style lang="scss" scoped>
@use '@/assets/css/vars' as vars;

.container {
  margin: 0 auto;
}

.route-path {
  color: vars.$gray;
}

.page-title {
  display: flex;
  font-size: 2.4rem;
  align-items: center;
  font-weight: vars.$bold;
  font-family: Gilroy;
  margin-bottom: 28px;
}

.buyer-data {
  &__fieldset {
    border: none;
    margin-bottom: 20px;
  }

  &__legend {
    font-weight: vars.$regular;
    font-family: Gilroy;
    margin-bottom: 20px;
    font-size: 1.8rem;
  }

  &__input {
    width: 100%;
    margin-bottom: 16px;
  }
}

.credit-card {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;

  &__number {
    width: calc(52% - 32px);
  }

  &__date,
  &__cvv {
    width: 24%;
  }
}

.order {
  &__summary {
    margin-bottom: 56px;
  }

  &__title {
    font-size: 1.8rem;
    margin-bottom: 28px;
    font-weight: bold;
  }

  &__row {
    display: flex;
    justify-content: space-between;
    margin-bottom: 12px;
  }

  &__subtotal {
    color: vars.$dark-gray;
  }

  &__delivery {
    color: vars.$dark-gray;
  }

  &__total {
    padding-top: 12px;
    border-top: 2px solid vars.$dark-cream;
    margin-bottom: 28px;
    font-weight: bold;
    font-size: 2rem;
  }
}

/*------------------------------------------------
  MEDIA QUERIES
------------------------------------------------*/
@media screen and (min-width: 640px) {
  .container {
    padding: 56px;
  }

  .route-path {
    margin-bottom: 20px;
  }

  .page-title {
    font-size: 3.4rem;
    margin-bottom: 28px;
  }

  .buyer-data {
    &__fieldset {
      margin-bottom: 40px;
    }

    &__legend {
      font-size: 2rem;
      margin-bottom: 24px;
    }

    &__input {
      width: calc(50%);
      display: inline-block;

      &:nth-child(even) {
        width: calc(50% - 28px);
        margin-right: 28px;
      }
    }
  }

  .credit-card {
    display: flex;
    align-items: flex-end;
    justify-content: space-between;

    &__number,
    &__date,
    &__cvv {
      &:nth-child(1) {
        width: calc(60% - 56px);
      }
      &:nth-child(2) {
        width: 20%;
        margin-right: 0;
      }

      &:nth-child(3) {
        width: 20%;
        margin-right: 0;
      }
    }
  }
}

@media screen and (min-width: 900px) {
  .buyer-data {
    display: inline-block;
    width: calc(70% - 80px);
    margin-right: 80px;
  }

  .order {
    vertical-align: top;
    display: inline-block;
    width: 30%;
  }
}
</style>