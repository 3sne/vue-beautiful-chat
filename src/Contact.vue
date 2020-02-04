<template>
  <div class="sc-contact" @click="$emit('clickContact', getParticipantInfo())">
    <div class="sc-contact--name">{{contactDetails.name}}</div>
    <div class="sc-contact--msg">{{getRecentMessage()}}</div>
  </div>
</template>

<script>
export default {
  props: {
    contactDetails: {
      type: Object,
      default: () => ({
        id: '0',
        name: 'Contact Z',
        phone: '522'
      })
    },
    recentMessage: {
      type: String,
      default: 'This is a temp message. ignore it. or dont. it doesn\'t matter'
    }
  },
  methods: {
    getRecentMessage() {
      return this.recentMessage;
    },
    getParticipantInfo() {
      return {
        id: "test_receiver",
        name: this.contactDetails.name,
        imageUrl: "https://avatars3.githubusercontent.com/u/1915989?s=230&v=4",
        phone: this.contactDetails.phone
      }
    }
  }
}
</script>

<style>
.sc-contact {
  margin: 15px;
}

.sc-contact:hover {
  cursor: pointer;
}

.sc-contact--name {
  font-weight: bold;
  font-size: 1.1em;
  text-transform: capitalize;
}

.sc-contact--msg {
  overflow: hidden;
  max-height: 1.5em;
  font-size: small;
}
</style>