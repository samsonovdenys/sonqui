<template>
    <form @submit.prevent="onSubmit">
        <input type="text" v-model="nick">
        <button type="submit">Create</button>
    </form>
</template>

<script>
export default {
    // data() takes some data from v-model in <template>
    data() {
        return {
            nick:''
        }
    },
    methods: {
        onSubmit() {
            console.log(this.nick)
            if(this.nick.trim()){
                const newUser = {
                    id: Date.now(),
                    nick: this.nick,
                    status: 'want_go_out',
                    active: false
                }

                this.$emit('add-user', newUser)
                this.nick = ''
            }
        }
    }
}
</script>

<style scoped>
    form{
        display:flex;
    }
    input{
        width: 400px;
    }
</style>