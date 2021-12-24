<template>
  <div class="newmail-display">

	<div class="container-contact100">
		<div class="wrap-contact100">
				<div class="wrap-input100 validate-input bg1" data-validate="Please Type Your Name">
					<span class="label-input100">From </span>
					<input class="input100" type="text" name="email" value="thPch@mail.com" disabled>
				</div>

				<div class="wrap-input100 validate-input bg1" data-validate="Please Type Your Name">
					<span class="label-input100">To </span>
					<input class="input100" type="text" name="email" placeholder="Enter Your Email ">
				</div>

				<div class="wrap-input100 validate-input bg0 rs1-alert-validate" data-validate = "Please Type Your Message">
					<span class="label-input100">Message</span>
					<textarea class="input100" name="message" placeholder="Your message here..."></textarea>
				</div>

				<div class="container-contact100-form-btn">
					<button class="contact100-form-btn">
						<span>
							Send
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

  export default {
    setup(props, {emit}){
      // let email = props.email;
      let sendEmail = () => { emit('sendEmail', {sent: true, save: true, closeModal: true})}

      useKeydown([
        {key: 'enter', fn: sendEmail},
      ])

      return {
        format,
        sendEmail
      }
    }
  }
</script>

<style scoped>
  @import '../assets/styles/newmail.css';
</style>
