<template>
  <InputEditableList
    v-model="items"
    :title="title"
    :max-items="maxItems"
    :show-count="showCount"
    :readonly="readonly"
    :required="required"
    :helper-text="helperText"
    :is-invalid="isInvalid"
    :no-items-message="$t('TRANSACTION.MULTI_PAYMENT.NO_RECIPIENTS')"
    class="TransactionMultiPaymentList"
    @remove="emitRemove"
  >
    <div
      slot-scope="{ item }"
      class="flex flex-1"
    >
      <WalletIdenticon
        :value="item.address || item.recipientId"
        :size="50"
        class="flex-inline"
      />

      <div class="flex-1 px-4">
        <div class="TransactionMultiPaymentList__recipient py-1">
          <span class="font-bold">
            {{ $t('TRANSACTION.RECIPIENT') }}:
          </span>

          <span>
            {{ formatWalletAddress(item.address || item.recipientId) }}
          </span>
        </div>

        <div class="TransactionMultiPaymentList__amount py-1">
          <span class="font-bold">
            {{ $t('TRANSACTION.AMOUNT') }}:
          </span>

          <span>
            {{ formatter_networkCurrency(item.amount) }}
          </span>
        </div>
      </div>
    </div>
  </InputEditableList>
</template>

<script>
import truncate from '@/filters/truncate'
import { InputEditableList } from '@/components/Input'
import WalletIdenticon from '@/components/Wallet/WalletIdenticon'

export default {
  name: 'TransactionMultiPaymentList',

  components: {
    InputEditableList,
    WalletIdenticon
  },

  props: {
    title: {
      type: String,
      required: false,
      default: function () {
        return this.$t('TRANSACTION.MULTI_PAYMENT.RECIPIENTS')
      }
    },

    items: {
      type: Array,
      required: true
    },

    maxItems: {
      type: Number,
      required: false,
      default: null
    },

    showCount: {
      type: Boolean,
      required: false,
      default: false
    },

    readonly: {
      type: Boolean,
      required: false,
      default: false
    },

    required: {
      type: Boolean,
      required: false,
      default: false
    },

    helperText: {
      type: String,
      required: false,
      default: null
    },

    isInvalid: {
      type: Boolean,
      required: false,
      default: false
    }
  },

  methods: {
    formatWalletAddress (address) {
      const walletName = this.wallet_name(address)
      if (walletName && walletName !== address) {
        address = `${truncate(walletName, 25)} (${this.wallet_truncate(address)})`
      }

      return address
    },

    emitRemove (index) {
      this.$emit('remove', index)
    }
  }
}
</script>
