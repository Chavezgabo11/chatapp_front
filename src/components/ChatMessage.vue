<template>
    <div class="message-panel" 
    :class="{ 'message-right' : messageRight }"
    :disabled="messageRight">
        <div class="message-wrapper">
            <h1>{{ username }}</h1>
            <p class="msg-text">
                {{ message }}
            </p>
            <p class="time">{{ currentDateTime() }}</p>
        </div>
    </div>
</template>

<script>

export default {
    name: 'ChatMessageComponent',

    props: {
        key: String,
        message: String,
        username: String,
        messageUser: String,
        idKey: String,
        socketID: String
    },
    methods: {  
        currentDateTime() {
            const current = new Date();
            const hour = current.getHours();
            const getTime = current.getHours() + ":" + current.getMinutes() + " " ;
            let timeAm = hour >= 12 ? "PM" : "AM";
            const dateTime = getTime + timeAm;
            return dateTime;
        }
    },
    mounted: function () {
      this.time = this.currentDateTime();
    },
    computed: {
            messageRight: function() {
                console.log(this.key);
                console.log(this.idKey);
                return (this.username === this.messageUser && this.socketID === this.idKey);
            }
    },
}
</script>

<style lang="scss">
    @import "@/assets/sass/_messages.scss";
</style>
