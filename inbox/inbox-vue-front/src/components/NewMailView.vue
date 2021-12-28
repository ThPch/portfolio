<template>
  <div class="newmail-display">

	<div class="container-contact100">
		<div class="wrap-contact100">
				<div class="wrap-input100 validate-input bg1" data-validate="Please Type Your Name">
					<span class="label-input100">From </span>
					<input class="input100" type="text" name="fromMail" v-model="fromMail" disabled >
				</div>

				<div class="wrap-input100 validate-input bg1" data-validate="Please Type Your Name">
					<span class="label-input100">To </span>
					<input class="input100" type="text" name="toMail" v-model="toMail" placeholder="Enter Your Email ">
				</div>

        <div class="wrap-input100 validate-input bg1" data-validate="Please Type your Subject">
					<span class="label-input100">Subject </span>
					<input class="input100" type="text" name="email" v-model="subject" placeholder="Subject ">
				</div>

				<div class="wrap-input100 validate-input bg0 rs1-alert-validate" data-validate = "Please Type Your Message">
					<span class="label-input100">Message</span>
					<textarea class="input100" name="message" v-model="body" placeholder="Your message here..." style="resize: none;"></textarea>
				</div>

				<div class="container-contact100-form-btn">
					<button class="contact100-form-btn" @click="sendEmailBtn">
						<span>
							Send to {{toMail}}
						</span>
					</button>
				</div>
		</div>
	</div>

  </div>
</template>

<script>
  import  { format } from 'date-fns'
  import useKeydown from '../composables/use-keydown'
  import {ref, getCurrentInstance } from 'vue'

  export default {
    setup(props, {emit}){
      let fromMail = ref("thPch@mail.com")
      let toMail = ref(null)
      let subject = ref(null)
      let body = ref(null)


      let sentActionDone = () => {

        let mail = {
          from : fromMail.value,
          to : toMail.value,
          subject : subject.value,
          body : body.value,
          sentAt : new Date(),
          sent : true,
        }

        emit('sentActionDone', {mail: mail, closeModal: true})
      }

      let closeModal = () => { emit('sentActionDone', {closeModal: true})}

      useKeydown([
        {key: 'Enter', fn: sentActionDone},
        {key: 'Escape', fn: closeModal},
      ])


      return {
        //Do not forget to return the setup function you want to use in methods API
        fromMail,
        toMail,
        subject,
        body,
        useKeydown,
        emit,
        closeModal,
        sentActionDone,
      }
    },

    components: {
      useKeydown,
    },
    props: {
      email: {
        type: Array,
        required: true
      }
    },
    methods: {
      sendEmailBtn() {
         this.sentActionDone()
      }
    }
  }
</script>

<style scoped>
  @import '../assets/styles/newmail.css';
</style>
