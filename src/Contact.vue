<template>
  <div class="sc-contact" @click="$emit('clickContact', getParticipantInfo())">
    <p class="sc-contact--name">
      {{`${contactDetails.phone}`}}
    </p>
    <p class="sc-contact--timestamp">
      {{getTimeStamp()}}
    </p>
    <div class="sc-contact--message">
      <span>
        {{getRecentMessage()}}
      </span>
    </div>
    <p class="sc-contact--pending"></p>
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
    },
    timestamp: {
      type: String
    }
  },
  methods: {
    getRecentMessage() {
      return this.recentMessage;
    },
    getTimeStamp() {
      const timestamp = this.timestamp;
      const current = new Date();
      let dTime = new Date(timestamp.replace(" ", "T"));
      let onlyTime = dTime.toLocaleTimeString(['en-US'], {
        hour: '2-digit', 
        minute: '2-digit'
      })
      let onlyDate = dTime.toLocaleDateString('default', {
        day: 'numeric',
        month: 'numeric',
        year: 'numeric'
      })

      let finalRender;
      if (current.getDate() === dTime.getDate()) {
        finalRender = `${onlyTime}`
      } else {
        finalRender = `${onlyDate}`
      }

      return finalRender;
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
  padding: 10px;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: auto auto;
  grid-template-areas:
    "name name timestamp timestamp"
    "message message message pending";
  grid-column-gap: 10px;
  align-items: center;
}
.sc-contact:hover {
  cursor: pointer;
}
.sc-contact > p  {
  margin: 0;
}

.sc-contact--name {
  grid-area: name;
  font-weight: bold;
  font-size: x-large;
  text-transform: capitalize;
}

.sc-contact--timestamp {
  grid-area: timestamp;
  justify-self: end;
  align-self: start;
  font-size: small;
}

.sc-contact--message {
  grid-area: message;
  align-self: center;
  justify-self: start;
  font-size: medium;
  height: 1.4em;
  line-height: 1.4rem;
  max-width: 120%;
}

.sc-contact--message > span {
  text-overflow: ellipsis;
  display: block;
  overflow: hidden;
  white-space: nowrap;
  min-width: 0;
}

.sc-contact--pending {
  grid-area: pending;
  align-self: center;
  justify-self: end;
}
</style>