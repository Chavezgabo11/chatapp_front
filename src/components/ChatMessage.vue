<template>
    <div class="message-panel" 
    :class="{ 'message-right' : messageRight }"
    :disabled="messageRight">
        <div class="message-wrapper">
            <h1>{{ username }}</h1>
            <p class="msg-text">
                {{ message }}
            </p>
        </div>
    </div>
</template>

<script>
import Chat from "@/views/Chat.vue";

export default {
    name: 'ChatMessageComponent',

    props: {
        message: String,
        username: String
    },

    // computed: {
    //     messageRight: function() {
    //         return (this.username.trim() == this.ChatUserName.trim());
    //         }
    // }
}
</script>

<style lang="scss">
    @import "@/assets/sass/_messages.scss";
</style>
