<template>
    <div>
        <!-- <div v-for="item in pdata" class="inline childbar">
            {{item.name}}
        </div> -->
    </div>
</template>
<script>
export default {
    data:function(){
        return{
            pdata:[
                {name:'name1',count:'count1'},{name:'name2',count:'count2'},{name:'name3',count:'count3'}
            ],
        }
    }
}
</script>
<style scoped>
    .inline{
        display: inline;
    }
    .childbar{

    }
</style>