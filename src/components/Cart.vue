<template>
  <div class="cart">
    <h1> {{ $t('cart.title') }} </h1>

    <table class="cart__contents">
      <thead>
        <tr>
          <th> # </th>
          <th> {{ $t('cart.productName') }} </th>
          <th> {{ $t('cart.productPrice') }} </th>
          <th> {{ $t('cart.productQuantity') }} </th>
          <th> {{ $t('cart.productTotal') }} </th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="(item, index) in items"
          :key="item.id"
        >
          <td> {{ index + 1 }} </td>
          <td> {{ item.name }} </td>
          <td>
            {{ $n(convertPrice(item.price), 'currency') }}
          </td>
          <td> {{ item.quantity }} </td>
          <td>
            {{ $n(convertPrice(item.quantity * item.price), 'currency') }}
          </td>
        </tr>

        <tr>
          <td />
          <td />
          <td />
          <td>
            <b> {{ $t('cart.orderTotal') }} </b>
          </td>

          <td>
            {{ $n(convertPrice(totalCost), 'currency') }}
          </td>
        </tr>
      </tbody>
    </table>

    <p v-html="arrivalEstimation" />

    <div class="cart__submit">
      <BaseButton
        variant="primary"
      >
        {{ $t('cart.goToPayment') }}
      </BaseButton>
    </div>
  </div>
</template>

<script>
import BaseButton from '@/components/BaseButton.vue';

export default {
  components: {
    BaseButton
  },

  computed: {
    /**
     *  MOCKED 'API' DATA
     */

    dollarToPln () {
      return 4.3; //totally correct & up-to-date exchange rate
    },

    items () {
      return [
        {
          id: 1,
          name: 'Vue T-Shirt - XL',
          quantity: 2,
          price: 19.99
        },
        {
          id: 2,
          name: 'Duck plushie',
          quantity: 12,
          price: 14.99
        },
        {
          id: 3,
          name: 'Vue coffee mug - white',
          quantity: 1,
          price: 9.99
        }
      ];
    },

    arrivalDate () {
      // 6th september 2022
      return new Date(2022, 8, 6);
    },

    /****/

    totalCost () {
      return this.items.reduce((sum, item) => {
        return sum + item.price * item.quantity;
      }, 0);
    },

    arrivalEstimation () {
      const date = this.$d(this.arrivalDate);

      return this.$t('cart.arrivalDate', { date });
    }
  },

  methods: {
    convertPrice (price) {
      return this.$i18n.locale === 'en'
        ? price
        : price * this.dollarToPln;
    }
  }
}
</script>

<style lang="scss">
.cart {
  th {
    text-align: left;
    background-color: var(--text-color);
    color: var(--white);
  }

  th, td {
    padding: 0.4rem;
    border: 0.1rem solid var(--border-color);

    &:empty {
      border: none;
    }
  }

  &__contents {
    width: 100%;
    border-spacing: 0;
    border-collapse: collapse;
  }

  &__submit {
    text-align: right;
  }
}
</style>
