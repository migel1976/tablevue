<template>
    <h1>
        {{header}}
    </h1>
</template>

<script>
    export default{
        name:'Header',
        props:{
            header:{
                type:String
            }
        }
    }
</script>
<style>
    h1{
        color:red
    }
</style>
