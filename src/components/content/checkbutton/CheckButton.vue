<template>
    <div class="check-button">
        <img src="~assets/img/cart/tick.svg" alt="" key="" :class="{check:isChecked}">
    </div>
</template>

<script>
export default {
    name:"CheckButton",
    props:{
        isChecked:{
            type:Boolean,
            default:true
        }
    }
}
</script>
<style scoped>
   .check-button img{
       border-radius: 50%;
       border: 2px solid #aaa;
   }

   .check{
       border-color: red;
       background-color: red;
   }

</style>