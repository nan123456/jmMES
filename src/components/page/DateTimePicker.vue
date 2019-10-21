<template>
    <div>
        <el-date-picker
        v-model="DateData"
        type="daterange"
        align="right"
        unlink-panels
        range-separator="至"
        start-placeholder="开始日期"
        end-placeholder="结束日期"
        ref="Date"
        value-format="yyyy-MM-dd"
        >
        </el-date-picker>
    </div>
</template>

<script>
export default {
    name:"DateTimePicker",
    data(){
        return{
            DateData:""
        }
    },
}
</script>