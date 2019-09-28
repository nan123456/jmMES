<template>
    <div class="demo-image__lazy">
        <img v-for="(value,index) in urls" :key="index" :src="value" style="width:50%;height:50%">
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
            this.urls.push('http://47.106.161.130:8081/jmmes/app/uploadfiles/'+url_arr[i])
        }
        console.log(this.urls)
    },
}
</script>

<style>

</style>