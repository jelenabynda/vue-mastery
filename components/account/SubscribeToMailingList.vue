<template lang='pug'>
form.form.card
  .card-body
    .form-group.-switched
      .switch
        input(id='subscribeSwitch' type='checkbox' v-model='subscribedToMailingList' @change='subscribe')
        label(for='subscribeSwitch')

      span I want to receive occasional emails about new educational content.

    .form-error
      .callout.-error(v-if='formError.length > 0' v-text='formError')
      .callout.-success(v-if='formSuccess.length > 0' v-text='formSuccess')
</template>

<script>
export default {
  name: 'account-subscribe-to-mailing-list',

  props: {
    account: {
      type: Object,
      required: true
    }
  },

  data () {
    return {
      subscribedToMailingList: this.account.subscribedToMailingList,
      debounceTimer: setTimeout(() => {}),
      formError: '',
      formSuccess: ''
    }
  },

  methods: {
    resetFormMessages () {
      this.formSuccess = this.formError = ''
    },

    subscribe () {
      this.resetFormMessages()
      clearTimeout(this.debounceTimer)
      this.debounceTimer = setTimeout(() => {
        this.$store.dispatch('userUpdateSubscription', this.subscribedToMailingList)
          .then(() => {
            this.formSuccess = 'Successfully updated your account details'
          })
          .catch((err) => {
            this.formError = 'Error saving the profile changes'
            console.error(err)
          })
      }, 500)
    }
  }
}
</script>

<style lang='stylus' scoped>
.form-group.-switched
  display flex
  flex-direction: row
  align-items center
</style>
