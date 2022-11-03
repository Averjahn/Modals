<template>
<div class="wrapper">

    <div id="wrapper-content">

      <section>
        <div class="container">
          
          <!-- modal with validate -->
          <button class="btn btnPrimary" @click="modalValidate = !modalValidate">Show modal window with validate</button>
          <modalValidate v-show="modalValidate" @close="modalValidate = false" regAuth/>

        </div>
      </section>

    </div>

  </div>
</template>

<script>

import modals from '@/components/UI/Modal.vue'
import modalValidate from '@/components/ModalValidate.vue'
export default {
  components: {
    modals,
    modalValidate,
  },
  data () {
    return {
      modalFirst: false,
      modalSecond: {
        show: false,
        name: '',
        email: '',
        password: '',
        passwordCo: ''
      },
      modalValidate: false,
      // regAuth: false
    }
  },
  methods: {
    submitSecondForm () {
      console.log({
        name: this.modalSecond.name,
        email: this.modalSecond.email 
      })
      this.modalSecond.name = '',
      this.modalSecond.email = ''
    }
  }
}

</script>

<style>
</style>
