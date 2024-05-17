<template>
    <div id="feedback" :class="tipoStatus">
        <p>{{msg}}</p>
    </div>
</template>
<script>
export default {
    name:'Message',
    props:{
        msg: String,
        tipoStatus: String
    }
}
</script>
<style scoped>
    .delete{
        background: rgb(90, 21, 21);
        color: white;
    }
    .update{
        background: rgb(5, 47, 109);
        color: white;
    }
    .create{
        background: rgb(4, 77, 2);
        color: white;
    }
    #feedback{
        position: absolute;
        bottom:100px;
        left:100px;
        padding: 15px;
        width: 200px;
        border-radius: 2px; 
        z-index: 3;       
    }
</style>