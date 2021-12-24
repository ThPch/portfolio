<template>
  <div class="bulk-action-bar">
    <span class="checkbox">
      <input type="checkbox"
            :checked="allEmailsSelected"
            :class="[someEmailsSelected ? 'partial-check' : '']"
            @click="bulkSelect" />
    </span>
    <span class="buttons">
      <button @click="compose">
        Compose
      </button>
      <button @click="emailSelection.markRead"
              :disabled="[...emailSelection.emails].every(e => e.read)">
        Mark Read
      </button>
      <button @click="emailSelection.markUnread"
              :disabled="[...emailSelection.emails].every(e => !e.read)">
        Mark Unread
      </button>
      <button @click="emailSelection.archive"
              :disabled="numberSelected === 0">
        Archive
      </button>
    </span>
  </div>

  <ModalView v-if="newMail" @closeModal="openedEmail = null">
    <!-- <NewMailView :email="openedEmail" @changeEmail="changeEmail" /> -->
    <NewMailView/>
  </ModalView>
</template>


<script>
  import ModalView from '@/components/ModalView.vue';
  import NewMailView from '@/components/NewMailView.vue';
  import MailView from '@/components/MailView.vue';
  import useEmailSelection from '@/composables/use-email-selection';
  import { computed, ref } from 'vue';

  export default {
    setup(props){
      let emailSelection = useEmailSelection();
      let numberSelected = computed(() => emailSelection.emails.size)
      let numberEmails = computed(() => props.emails.length)
      let allEmailsSelected = computed(() => numberSelected.value === numberEmails.value)
      let someEmailsSelected = computed(() => {
        return numberSelected.value > 0 && numberSelected.value < numberEmails.value
      })
      let bulkSelect = function(){
        if(allEmailsSelected.value) {
          emailSelection.clear()
        } else {
          emailSelection.addMultiple(props.emails)
        }
      }
      return {
        ModalView,
        MailView,
        NewMailView,
        allEmailsSelected,
        someEmailsSelected,
        bulkSelect,
        emailSelection,
        numberSelected,
        newMail: ref(false),
        openedEmail: ref(null)
      }
    },
    components: {
      NewMailView,
      ModalView,
    },
    props: {
      emails: {
        type: Array,
        required: true
      }
    },
    methods: {
      compose() {
        console.log("compose button is pressed")
        this.newMail = true
        this.openedEmail = true
      }
    }
  }
</script>

<style scoped>

</style>



