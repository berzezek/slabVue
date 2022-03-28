<template>
  <VueRecaptcha ref="recaptcha" sitekey="Your key here" />
  <h1>Hello</h1>
</template>

<script>
  import { VueRecaptcha } from 'vue-recaptcha';
  export default {
    name: 'VueRecaptcha',
    data: () => ({
    sitekey: '6LeIxAcTAAAAAJcZVRqyHh71UMIEGNQ_MXjiZKhI',
    }),
    components: {
        VueRecaptcha,
    },
    methods: {
        onSubmit: function () {
        this.$refs.invisibleRecaptcha.execute()
        },
        onVerify: function (response) {
        console.log('Verify: ' + response)
        },
        onExpired: function () {
        console.log('Expired')
        },
        resetRecaptcha() {
        this.$refs.recaptcha.reset() // Direct call reset method
        },
    },
  };
</script>