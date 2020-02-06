<template>
  <div class="sc-contact-list">
    <div class="sc-header" :style="{background: colors.header.bg, color: colors.header.text}">
        <div class="sc-header--title enabled">Your Conversations</div>
    </div>
    <div class="sc-contact-list--body"> 
      <!-- <Contact 
        v-for="(contact, index) in contactList"
        :key="index"
        :contactDetails="contact.contactDetails"
        :recentMessage="contact.recentMessage"
      /> -->

      <div v-for="(contact, index) in contactList" :key="index">
        <Contact 
          :contactDetails="contact.contactDetails"
          :recentMessage="contact.recentMessage"
          :timestamp="contact.timestamp"
          @clickContact="clickContact"
        />
        <hr class="hr"/>
      </div>
      <!-- <div v-for="index in contactList.length" style="" :key="index">
        <Contact 
          contactName="Real working"
          contactPhone="13142077908"
          @clickContact="clickContact"
        />
        <hr class="hr"/>
        <Contact 
          contactName="Dummy nubmer 1"
          contactPhone="201"
          @clickContact="clickContact"
        />
        <Contact 
          contactName="Dummy nubmer 2"
          contactPhone="202"
          @clickContact="clickContact"
        />
      </div> -->
    </div>
  </div>
</template>
<script>
import Contact from './Contact.vue'
export default {
  components: {
    Contact
  },
  props: {
    show: {
      default: true,
      type: Boolean
    },
    colors: {
      type: Object,
      required: true
    },
    contactList: {
      type: Array,
      default: () => []
    }
  },

  methods: {
    clickContact(participant) {
      this.$emit('clickContact', participant)
    }
  }
}
</script>
<style>
.sc-contact-list {
  display: flex;
  flex-direction: column;
  height: 100%;
  overflow-y: auto;
  min-width: 300px;
  background-size: 100%;
  background:white;
}
.sc-contact-list--body::-webkit-scrollbar {
  width: 11px;
}
.sc-contact-list--body::-webkit-scrollbar-track {
  background: rgb(255, 255, 255)
}
.sc-contact-list--body::-webkit-scrollbar-thumb {
  background-color: #90A4AE ;
  border-radius: 6px;
  border: 3px solid rgb(255, 255, 255);
}

.sc-contact-list--body {
  overflow-y: auto;
  scrollbar-width: thin;
  scrollbar-color:#90A4AE rgb(255, 255, 255);
}

.sc-header {
  min-height: 75px;
  padding: 10px;
  box-shadow: 0 1px 4px rgba(0,0,0,.2);
  position: relative;
  box-sizing: border-box;
  display: flex;
}
.sc-header--title {
  align-self: center;
  padding: 10px;
  flex: 1;
  user-select: none;
  font-size: 20px;
}

.hr {
  display: block;
  position: relative;
  margin: 0px 5px;
  border: 0;
  border-top: 1px solid #eee;
}

</style>