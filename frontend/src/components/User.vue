<template>
    <li>
        <span v-bind:class="{done: user.active}">
            <input type="checkbox"
                     v-on:change="user.active = !user.active">
            <strong>
                {{ user.title}}
                MY-STATUS IS: 
                {{ user.status }}
            </strong>
        </span>
        <!--Function $emit() Tells parent element if there are some events on element-->
        <button class="rm" v-on:click="$emit('remove-user', user.id)" >&times;</button>
    </li>
</template>

<script> 

export default {
    props:{
        user:{
            type: Object,
            required:true
        }
    }
}

</script>

<style>
    li{
        border: 1px solid #ccc;
        display: flex;
        justify-content: space-between;
        padding: .5rem 2rm;
        margin-bottom: 1rm;
        padding: 12px 50px;
    }
    .rm{
        background: rgb(216, 120, 120);
        color: #fff;
        border-radius: 50%;
        border-color: rgb(216, 120, 120);
        font-weight: bold;
    }
    input{
        margin-right: 1rem;
    }

    .done{
        text-decoration: underline;
    }
</style>