<template>
    <div style="overflow:auto;display:inline-block;height:3000px ">
        <img v-for="(value,index) in urls" :key="index" :src="value" style="width:400px;height:400px">
    </div>    
</template>

<script>

export default {
    name: 'Photo',
    data(){
        return{
            urls : []
        }
    },
    created() {
        var url_arr=this.$route.query.url.split(',');
        var i =0
        console.log(url_arr)
        for(i=0;i < url_arr.length;i++){
            this.urls.push('http://47.106.161.130:80/jmmes/app/uploadfiles/'+url_arr[i])
        }
        console.log(this.urls)
    },
}
</script>

<style scoped>

</style>