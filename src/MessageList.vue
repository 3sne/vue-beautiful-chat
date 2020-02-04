<template>
  <div class="sc-message-list" ref="scrollList" :style="{backgroundColor: colors.messageList.bg}" @scroll="handleScroll">
    <Message v-for="(message, idx) in messages" :message="message" :user="profile(message.author)" :key="idx" :colors="colors" :messageStyling="messageStyling" @remove="$emit('remove', message)">
      <template v-slot:user-avatar="scopedProps">
        <slot name="user-avatar" :user="scopedProps.user" :message="scopedProps.message">
        </slot>
      </template>
      <template v-slot:text-message-body="scopedProps">
        <slot name="text-message-body" :message="scopedProps.message" :messageText="scopedProps.messageText" :messageColors="scopedProps.messageColors" :me="scopedProps.me">
        </slot>
      </template>
      <template v-slot:text-message-toolbox="scopedProps">
        <slot name="text-message-toolbox" :message="scopedProps.message" :me="scopedProps.me">
        </slot>
      </template>
    </Message>
    <Message v-show="showTypingIndicator !== ''" :message="{author: showTypingIndicator, type: 'typing'}" :user="{}" :colors="colors" :messageStyling="messageStyling" />
  </div>
</template>
<script>
import Message from './Message.vue'
import chatIcon from './assets/chat-icon.svg'

export default {
  components: {
    Message
  },
  data() {
    return {
      _currScroll: null
    }
  },
  props: {
    participants: {
      type: Array,
      required: true
    },
    messages: {
      type: Array,
      required: true
    },
    showTypingIndicator: {
      type: String,
      required: true
    },
    colors: {
      type: Object,
      required: true
    },
    alwaysScrollToBottom: {
      type: Boolean,
      required: true
    },
    messageStyling: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    _doScroll () {
      if (!this._currScroll)
        this.$refs.scrollList.scrollTop = this.$refs.scrollList.scrollHeight
      else
        this.$refs.scrollList.scrollTop = this.$refs.scrollList.scrollHeight - this._currScroll
    },
    handleScroll (e) {
        if (e.target.scrollTop === 0) {
            this.$emit('scrollToTop')
        }
    },
    shouldScrollToBottom() {
      return this.alwaysScrollToBottom || (this.$refs.scrollList.scrollTop > this.$refs.scrollList.scrollHeight - 600)
    },
    profile(author) {
      const profile = this.participants.find(profile => profile.id === author)

      // A profile may not be found for system messages or messages by 'me'
      return profile || {imageUrl: '', name: ''}
    }
  },
  computed: {
    defaultChatIcon() {
      return chatIcon
    }
  },
  mounted () {
    this.$nextTick(this._doScroll())
  },
  beforeUpdate() {
    this._currScroll = this.$refs.scrollList.scrollHeight - this.$refs.scrollList.scrollTop;
  },
  updated () {
    if (this.shouldScrollToBottom())
      this.$nextTick(this._doScroll())
  }
}
</script>

<style scoped>
.sc-message-list {
  height: 80%;
  overflow-y: auto;
  background-size: 100%;
  padding: 20px 0px;
  scrollbar-width: thin;
  scrollbar-color:#90A4AE rgb(255, 255, 255);
}
.sc-message-list::-webkit-scrollbar {
  width: 11px;
}
.sc-message-list::-webkit-scrollbar-track {
  background: rgb(255, 255, 255)
}
.sc-message-list::-webkit-scrollbar-thumb {
  background-color: #90A4AE ;
  border-radius: 6px;
  border: 3px solid rgb(255, 255, 255);
}
</style>
