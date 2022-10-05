<template>
    <div>
        <ul>
            <User 
                v-for="user in users"
                v-bind:user="user"
                v-on:remove-user="removeUser"
            />
        </ul>
    </div>
</template>

<script>

import User from './User.vue'

export default {
    //props: is a method 'properties' to take data from App.vue v-bind 'key' and use it in <template> with {{}} or other vue methods like v-for ...
    props:['users'],
    components: {
        User
    },
    // All methods like v-on:remove-todo="removeTodo" are executed in 'methods:'
    methods: {
        removeUser(id){
            this.$emit('remove-user', id)
        }
    }
}

</script>

<!--'scoped' do use styles only for this component-->
<style scoped>
    ul {
        list-style: none;
        margin: 0;
        padding: 0;
    }
</style>