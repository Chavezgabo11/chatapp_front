<template>
    <main>
        <h1 class="welcome-message">Welcome {{ ChatUserName }} </h1>

        <section id="chat">
            <!-- the left-hand side of our UI where we see current users -->
            <section id="chat-users-ui">
                <h2>Current Users:</h2>
                <!-- list of users -->
                <ul id="current-users">
                    <!-- this will take a sub-component that will render -->
                    <ChatUsers
                    v-for="user in users"
                    :key="user.id"
                    :username="user.name"
                    />
                </ul>
            </section>
            <!-- end left hand -->
            <!-- main chat UI -->
            <section id="chat-messages-ui">
                <!-- custom chat message component here -->
                <ChatMessage
                    v-for="msg in messages"
                    :key="msg.id"
                    :message="msg.message"
                    :username="msg.user"
                    :messageUser="ChatUserName"
                    :idKey="socketID"
                    :socketID="msg.socketID"
                />

                <section id="text-wrapper">
                    <textarea @keyup="trySendMessage" id="message" v-model="message" placeholder="What's on your mind?"></textarea>

                    <button 
                    id="sendMessage"
                    @click="sendMessage"
                    :class="{ 'disabled' : canSend }"
                    :disabled="canSend"
                    >
                        <img id="icon-send" :src="require(`@/assets/icon-send.png`)" alt="Icon Send Message">
                    </button>
                </section>

            </section>
        </section>
    </main>
</template>
<script>
import io from "socket.io-client";
import vars from "@/env.js";
import ChatMessage from "@/components/ChatMessage.vue";
import ChatUsers from "@/components/ChatUsers.vue";

export default {
    name: "TheChatComponent",

    props: {
        ChatUserName: String
    },
    

    computed: {
            canSend: function() {
                return (this.message.trim() === "");
            },
            userMatch: function() {
                return true;
            }
    },

    mounted() {
        let vm = this;

        this.socket.on("CONNECTED", (id) => {
            vm.socketID = id;

            vm.broadCastJoined();
        })

        this.socket.on("JOINED", (data) => {
            vm.users = data;
        })

        this.socket.on('MESSAGE', (message) => {
            message.id = this.socketID;

            // [...]  is a the spread operator
            // short hand way to add something to an array, or put
            // 2 arrays together, etc
            vm.messages = [...vm.messages, message];
            console.log(message);
        })

        this.socket.on('SOMEONE_TYPING', (data) => console.log('someone is typing', data));
    },

    data() {
        return {
            socketID: '',
            MyId: '',
            time: '',
            users: [],
            UserNameForMessage: '',
            message: '',
            messages: [],
            CurrentID: 0,

            // set up the socket connection on the frontend 
            // this connects to the chat service running on port 3000
            socket: io(vars.basePath, {
                withCredentials: false,
                extraHeaders: {
                    'Access-Control-Allow-Origin' : '*'
                }
            })
        }
    },

    methods: {
        sendMessage(){
            this.socket.emit('SEND_MESSAGE', {user: this.ChatUserName || "Anonymous", message: this.message, socketID: this.socketID});

            // empty put the text area and get ready to input a new message
            this.message = '';    
        },
        trySendMessage(event) {
            console.log('typing a message');

            // if the enter is pushed AND canSend has a value, then call the sendMessage method
            if (event.keyCode === 13 && this.canSend === false) {
                this.sendMessage();
            } else {
                // else fire off teh USER_TYPING event to the Chat Server
                this.socket.emit('USER_TYPING', { user: this.username || "Anonymous"});
            }
        },
        
        getNewId() {
            this.CurrentID++;
            return this.CurrentID;
        }
        ,
        broadCastJoined() {
            this.socket.emit('JOINED_CHAT', { name: this.ChatUserName || 'Anonymous'})
        }
    },

    components: {
        ChatMessage,
        ChatUsers
    }
}
</script>

<style lang="scss">
    @import "@/assets/sass/_chat.scss";
</style>