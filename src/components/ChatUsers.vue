<template>
    <li class="user-panel">
        <span class="user-avatar">
            <img :src="require(`@/assets/user.png`)" alt="User Image">
        </span>
        <h3 class="user-name">{{ username }}</h3>
    </li>
</template>

<script>

export default {
    name: "ChatUser",
    props: {
        username: String
    }
}
</script>

<style lang="scss">
    .user-panel {
        display: flex;
        background-color: white;
        padding: 0.5em;
        border-radius: $rounded;
        margin-bottom: 10px;

        h3 {
            margin: 10px;
            border: 0;
            margin: 0;
            padding: 0;
            color: black;
            padding-top: 20px;
            padding-bottom: 20px;
            font-size: 20px;
            font-weight: bold;
            margin-left: 5px;
        }
    }

    .user-panel:hover {
        display: flex;
        background-color: white;
        padding: 0.5em;
        box-shadow: 0 2px 10px #ccc;
        border-radius: $rounded;
        margin-bottom: 10px;

        h3 {
            margin: 10px;
            border: 0;
            margin: 0;
            padding: 0;
            color: black;
            padding-top: 20px;
            padding-bottom: 20px;
            font-size: 20px;
            font-weight: bold;
            margin-left: 5px;
        }
    }
</style>