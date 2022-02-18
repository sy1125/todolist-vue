<template>
    <div class="hello">
        <div class="hello__title">What is your name?</div>
        <label class="hello__user-name" for="user-name" >Name</label>
        <input type="text" id="user-name" v-model="userName" v-on:keyup.enter="addUserName" />
    </div>
</template>

<script>
export default {
    data() {
        return {
            userName: ""
        };
    },
    methods: {
        addUserName() {
            this.$emit("addName", this.userName);
        }
    }
};
</script>
