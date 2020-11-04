<template>
    <div class="page-loader" v-if="!isLoaded">
        <div class="cube"></div>
        <div class="cube"></div>
        <div class="cube"></div>
        <div class="cube"></div>
    </div>
</template>


<script>

export default{
    data(){
        return{
            isLoaded: false
        }
    },
    mounted(){
        document.onreadystatechange = () =>{
            if(document.readyState == "complete"){
                setTimeout(() => {
                    this.isLoaded = true;
                }, 1000)

            }
        }

    }
}
</script>


