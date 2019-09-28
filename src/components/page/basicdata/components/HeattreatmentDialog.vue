<template>
    <div>
        <!-- 制造工艺 -->
        <el-dialog 
            title="产品热处理工艺技术要求及检验记录表"
            :visible.sync="dialogcraftsmanshipVisible"
            width="60%">
            <div slot="title" v-if="!craftsmanshipTableHeader.contactId" style="height:60px">
                <h3>产品热处理工艺技术要求及检验记录表</h3>
                <el-select v-model="value" placeholder="请选择" style="margin: 20px 0;" @change="model(value)">
                    <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                    </el-option>
                </el-select>
            </div>
            <table class="craftsmanshipTableHeader">
                <tr>
                <td>产品型号名称</td>
                <td><input v-model="craftsmanshipTableHeader.productName"/></td>
                <td>零件名称</td>
                <td><input v-model="craftsmanshipTableHeader.ownPartName"/></td>
                <td>工单号</td>
                <td><input v-model="craftsmanshipTableHeader.partsName"/></td>
                </tr>
                <tr>
                <td>零件图号</td>
                <td><input v-model="craftsmanshipTableHeader.productDrawingNumber"/></td>
                <td>数量</td>
                <td><input v-model="craftsmanshipTableHeader.ownPartDrawingNumber"/></td>
                <td>炉号</td>
                <td><input v-model="craftsmanshipTableHeader.partsDrawingNumber"/></td>
                </tr>
            </table>
            <table class="craftsmanshipTableHeader">
                <tr>
                    <td><h1>操&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;作&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;规&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;程&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{selectData.date}}</h1></td>
                </tr>
            </table>
            <table class="craftsmanshipTableHeader" v-if="model1">
                <tr>
                    <td>工序</td>
                    <td>子工序</td>
                    <td>要求</td>
                    <td>记录</td>
                    <td rowspan="2" style="width:30%;">工艺曲线</td>
               </tr>
                <tr>
                    <td rowspan="10">淬火</td>
                    <td>装炉</td>
                    <td>炉温≤450℃</td>
                    <td>实际炉温：{{temperature[0]}}℃<button class="updateBtn" @click="updateBtn(0,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="3">加热</td>
                    <td rowspan="2">请较准确地记录：加热升<br>温到淬火温度的时间</td>
                    <td>起始时间：{{time[0]}} 净时间<button class="updateBtn" @click="updateBtn(0,'time')">更新</button></td>
                    <td rowspan="17"></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[1]}}<button class="updateBtn" @click="updateBtn(1,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>加热温度850℃ ± 10°</td>
                    <td>实际温度：{{temperature[1]}}℃<button class="updateBtn" @click="updateBtn(1,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">保温</td>
                    <td rowspan="2">加热时间 ≈ aKD<br>(aKD计算方法参照：热处理规程中说明)</td>
                    <td>起始时间：{{time[2]}} 净时间<button class="updateBtn" @click="updateBtn(2,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[3]}}<button class="updateBtn" @click="updateBtn(3,'time')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">冷却</td>
                    <td>盐水浓度为5% - 10%</td>
                    <td>实际盐水浓度：<input class="OtherData" v-model="modelOther1.BrineStrength">%</td>
                </tr>
                <tr>
                    <td>盐水温度在淬火过程中 ＜ 50℃</td>
                    <td>实际盐水温度：<input class="OtherData" v-model="modelOther1.SaltwaterTemperature"> ℃</td>
                </tr>
                <tr>
                    <td>&nbsp;</td>
                    <td></td>
                    <td></td>
                </tr>
                <tr>
                    <td>硬度抽查</td>
                    <td>≥HRC43</td>
                    <td>
                    <input class="OtherData" v-model="modelOther1.hardness1">  
                    <input class="OtherData" v-model="modelOther1.hardness2">  
                    <input class="OtherData" v-model="modelOther1.hardness3">
                    </td>
                </tr>
                <tr>
                    <td rowspan="7">回火</td>
                    <td>装炉</td>
                    <td>炉温 ≤ 300℃</td>
                    <td>实际炉温：{{temperature[2]}}℃<button class="updateBtn" @click="updateBtn(2,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="3">加热</td>
                    <td rowspan="3">回火加热温度560℃左右<br>（注意：回火温度应参照淬火硬度<br>和图纸要求的调质硬度做适当调整） </td>
                    <td>起始时间：{{time[4]}} 净时间<button class="updateBtn" @click="updateBtn(4,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[5]}}<button class="updateBtn" @click="updateBtn(5,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>实际温度：{{temperature[3]}}℃<button class="updateBtn" @click="updateBtn(3,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">保温</td>
                    <td rowspan="2">约2 - 3小时</td>
                    <td>起始时间：{{time[6]}} 净时间<button class="updateBtn" @click="updateBtn(6,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[7]}}<button class="updateBtn" @click="updateBtn(7,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>冷却</td>
                    <td>空冷</td>
                    <td>注意：工件放置处应干燥无水迹</td>
                </tr>
                <tr>
                    <td colspan="4">&nbsp;</td>
                </tr>
            </table>
            <table class="craftsmanshipTableHeader"  v-if="model5">
                <tr>
                    <td>工序</td>
                    <td>子工序</td>
                    <td>要求</td>
                    <td>记录</td>
                    <td rowspan="2" style="width:30%;">工艺曲线</td>
               </tr>
                <tr>
                    <td rowspan="9">淬火</td>
                    <td>装炉</td>
                    <td>(易开裂零件)炉温≤300℃</td>
                    <td>实际炉温：{{temperature[0]}}℃<button class="updateBtn" @click="updateBtn(0,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="3">加热</td>
                    <td rowspan="2">请较准确地记录：加热<br>升温到淬火温度的时间</td>
                    <td>起始时间：{{time[0]}} 净时间<button class="updateBtn" @click="updateBtn(0,'time')">更新</button></td>
                    <td rowspan="15"></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[1]}}<button class="updateBtn" @click="updateBtn(1,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>加热温度870℃ ± 10°</td>
                    <td>实际温度：  {{temperature[1]}}℃<button class="updateBtn" @click="updateBtn(1,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">保温</td>
                    <td rowspan="2">加热时间 ≈ aKD<br>(aKD计算方法参照：热处理规程中说明)</td>
                    <td>起始时间：{{time[2]}} 净时间<button class="updateBtn" @click="updateBtn(2,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[3]}}<button class="updateBtn" @click="updateBtn(3,'time')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">冷却</td>
                    <td>PAG浓度为1.25%左右</td>
                    <td>实际PAG浓度：<input class="OtherData" v-model="modelOther5.PAGStrength"> %</td>
                </tr>
                <tr>
                    <td>PAG温度在淬火过程中 ＜ 40℃<br>在1.25%PAG冷至液温</td>
                    <td>实际PAG温度：<input class="OtherData" v-model="modelOther5.PAGTemperature"> ℃<br>PAG溶液品质：<input class="OtherData" v-model="modelOther2.PAGQuality"> </td>
                </tr>
                <tr>
                    <td>硬度抽查</td>
                    <td>≥HRC45</td>
                    <td><input class="OtherData" v-model="modelOther5.hardness1">  <input class="OtherData" v-model="modelOther5.hardness2">  <input class="OtherData" v-model="modelOther5.hardness3"></td>
                </tr>
                <tr>
                    <td rowspan="7">回火</td>
                    <td>装炉</td>
                    <td>炉温 ≤ 300℃</td>
                    <td>实际炉温：{{temperature[2]}}℃<button class="updateBtn" @click="updateBtn(2,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="3">加热</td>
                    <td rowspan="3">回火加热温度560℃左右<br>（注意：回火温度应参照淬火硬度<br>和图纸要求的调质硬度做适当调整） </td>
                    <td>起始时间：{{time[4]}} 净时间<button class="updateBtn" @click="updateBtn(4,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[5]}}<button class="updateBtn" @click="updateBtn(5,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>实际温度：{{temperature[3]}}℃<button class="updateBtn" @click="updateBtn(3,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">保温</td>
                    <td rowspan="2">约2 - 3小时</td>
                    <td>起始时间：{{time[6]}} 净时间<button class="updateBtn" @click="updateBtn(6,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[7]}}<button class="updateBtn" @click="updateBtn(7,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>冷却</td>
                    <td>空冷</td>
                    <td>注意：工件放置处应干燥无水迹</td>
                </tr>
                <tr>
                    <td colspan="5">&nbsp;</td>
                </tr>
            </table>
            <table class="craftsmanshipTableHeader" v-if="model3">
                <tr>
                    <td>工序</td>
                    <td>子工序</td>
                    <td>要求</td>
                    <td>记录</td>
                    <td rowspan="2" style="width:30%;">工艺曲线</td>
               </tr>
                <tr>
                    <td rowspan="9">淬火</td>
                    <td>装炉</td>
                    <td>(易开裂零件)炉温≤300℃</td>
                    <td>实际炉温：{{temperature[0]}}℃<button class="updateBtn" @click="updateBtn(0,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="3">加热</td>
                    <td rowspan="2">请较准确地记录：加热<br>升温到淬火温度的时间</td>
                    <td>起始时间：{{time[0]}} 净时间<button class="updateBtn" @click="updateBtn(0,'time')">更新</button></td>
                    <td rowspan="15"></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[1]}}<button class="updateBtn" @click="updateBtn(1,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>加热温度870℃ ± 10°</td>
                    <td>实际温度：  {{temperature[1]}}℃<button class="updateBtn" @click="updateBtn(1,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">保温</td>
                    <td rowspan="2">加热时间 ≈ aKD<br>(aKD计算方法参照：热处理规程中说明)</td>
                    <td>起始时间：{{time[2]}} 净时间<button class="updateBtn" @click="updateBtn(2,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[3]}}<button class="updateBtn" @click="updateBtn(3,'time')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">冷却</td>
                    <td>PAG浓度为1.25%左右</td>
                    <td>实际PAG浓度：<input class="OtherData" v-model="modelOther3.PAGStrength">%</td>
                </tr>
                <tr>
                    <td>PAG温度在淬火过程中 ＜ 40℃<br>在1.25%PAG冷至液温</td>
                    <td>实际PAG温度<input class="OtherData" v-model="modelOther3.PAGTemperature">℃<br>PAG溶液品质<input class="OtherData" v-model="modelOther3.PAGQuality"></td>
                </tr>
                <tr>
                    <td>硬度抽查</td>
                    <td>≥HRC45</td>
                    <td><input class="OtherData" v-model="modelOther3.hardness1">  <input class="OtherData" v-model="modelOther3.hardness2">  <input class="OtherData" v-model="modelOther3.hardness3"></td>
                </tr>
                <tr>
                    <td rowspan="7">回火</td>
                    <td>装炉</td>
                    <td>炉温 ≤ 300℃</td>
                    <td>实际炉温：{{temperature[2]}}℃<button class="updateBtn" @click="updateBtn(2,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="3">加热</td>
                    <td rowspan="3">回火加热温度560℃左右<br>（注意：回火温度应参照淬火硬度<br>和图纸要求的调质硬度做适当调整） </td>
                    <td>起始时间：{{time[4]}} 净时间<button class="updateBtn" @click="updateBtn(4,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[5]}}<button class="updateBtn" @click="updateBtn(5,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>实际温度：{{temperature[3]}}℃<button class="updateBtn" @click="updateBtn(3,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">保温</td>
                    <td rowspan="2">约2 - 3小时</td>
                    <td>起始时间：{{time[6]}} 净时间<button class="updateBtn" @click="updateBtn(6,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[7]}}<button class="updateBtn" @click="updateBtn(7,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>冷却</td>
                    <td>空冷</td>
                    <td>注意：工件放置处应干燥无水迹</td>
                </tr>
                <tr>
                    <td colspan="5">&nbsp;</td>
                </tr>
            </table>

            <table class="craftsmanshipTableHeader" v-if="model6">
                <tr>
                    <td>工序</td>
                    <td>子工序</td>
                    <td>要求</td>
                    <td>记录</td>
                    <td rowspan="2" style="width:30%;">工艺曲线</td>
               </tr>
                <tr>
                    <td rowspan="9">淬火</td>
                    <td>装炉</td>
                    <td>(易开裂零件)炉温≤300℃</td>
                    <td>实际炉温：{{temperature[0]}}℃<button class="updateBtn" @click="updateBtn(0,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="3">加热</td>
                    <td rowspan="2">请较准确地记录：加热<br>升温到淬火温度的时间</td>
                    <td>起始时间：{{time[0]}} 净时间<button class="updateBtn" @click="updateBtn(0,'time')">更新</button></td>
                    <td rowspan="15"></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[1]}}<button class="updateBtn" @click="updateBtn(1,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>加热温度870℃ ± 10°</td>
                    <td>实际温度：  {{temperature[1]}}℃<button class="updateBtn" @click="updateBtn(1,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">保温</td>
                    <td rowspan="2">加热时间 ≈ aKD<br>(aKD计算方法参照：热处理规程中说明)</td>
                    <td>起始时间：{{time[2]}} 净时间<button class="updateBtn" @click="updateBtn(2,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[3]}}<button class="updateBtn" @click="updateBtn(3,'time')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">冷却</td>
                    <td>PAG浓度为1.25%左右</td>
                    <td>实际PAG浓度：<input class="OtherData" v-model="modelOther6.PAGStrength">%</td>
                </tr>
                <tr>
                    <td>PAG温度在淬火过程中 ＜ 40℃<br>在1.25%PAG冷至液温</td>
                    <td>实际PAG温度：<input class="OtherData" v-model="modelOther6.PAGTemperature">℃<br>PAG溶液品质：<input class="OtherData" v-model="modelOther6.PAGQuality"></td>
                </tr>
                <tr>
                    <td>硬度抽查</td>
                    <td>≥HRC45</td>
                    <td><input class="OtherData" v-model="modelOther6.hardness1">  <input class="OtherData" v-model="modelOther6.hardness2">  <input class="OtherData" v-model="modelOther6.hardness3"></td>
                </tr>
                <tr>
                    <td rowspan="7">回火</td>
                    <td>装炉</td>
                    <td>炉温 ≤ 300℃</td>
                    <td>实际炉温：{{temperature[2]}}℃<button class="updateBtn" @click="updateBtn(2,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="3">加热</td>
                    <td rowspan="3">回火加热温度560℃左右<br>（注意：回火温度应参照淬火硬度<br>和图纸要求的调质硬度做适当调整） </td>
                    <td>起始时间：{{time[4]}} 净时间<button class="updateBtn" @click="updateBtn(4,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[5]}}<button class="updateBtn" @click="updateBtn(5,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>实际温度：{{temperature[3]}}℃<button class="updateBtn" @click="updateBtn(3,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">保温</td>
                    <td rowspan="2">约2 - 3小时</td>
                    <td>起始时间：{{time[6]}} 净时间<button class="updateBtn" @click="updateBtn(6,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[7]}}<button class="updateBtn" @click="updateBtn(7,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>冷却</td>
                    <td>空冷</td>
                    <td>注意：工件放置处应干燥无水迹</td>
                </tr>
                <tr>
                    <td colspan="5">&nbsp;</td>
                </tr>
            </table>

            <table class="craftsmanshipTableHeader" v-if="model2">
                <tr>
                    <td>工序</td>
                    <td>子工序</td>
                    <td>要求</td>
                    <td>记录</td>
                    <td rowspan="2"  style="width:30%;">工艺曲线</td>
               </tr>
                <tr>
                    <td rowspan="10">淬火</td>
                    <td>装炉</td>
                    <td>高合金钢：炉温≤300℃</td>
                    <td>实际炉温：{{temperature[0]}}℃<button class="updateBtn" @click="updateBtn(0,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="3">加热</td>
                    <td rowspan="2">请较准确地记录：加热<br>升温到淬火温度的时间</td>
                    <td>起始时间：{{time[0]}} 净时间<button class="updateBtn" @click="updateBtn(0,'time')">更新</button></td>
                    <td rowspan="17"></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[1]}}<button class="updateBtn" @click="updateBtn(1,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>加热温度970℃ ± 10°</td>
                    <td>实际温度：  {{temperature[1]}}℃<button class="updateBtn" @click="updateBtn(1,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">保温</td>
                    <td rowspan="2">加热时间 ≈ aKD<br>(aKD计算方法参照：热处理规程中说明)</td>
                    <td>起始时间：{{time[2]}} 净时间<button class="updateBtn" @click="updateBtn(2,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[3]}}<button class="updateBtn" @click="updateBtn(3,'time')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">冷却</td>
                    <td>空气</td>
                    <td>实际空气温度：<input class="OtherData" v-model="modelOther2.airtemperature"> ℃</td>
                </tr>
                <tr>
                    <td>冷却环境干燥无水迹等</td>
                    <td>实际条件：<input class="OtherData" v-model="modelOther2.Actual"></td>
                </tr>
                <tr>
                    <td>&nbsp;</td>
                    <td></td>
                    <td></td>
                </tr>
                <tr>
                    <td>硬度抽查</td>
                    <td>≥HRC38</td>
                    <td><input class="OtherData" v-model="modelOther2.hardness1">  <input class="OtherData" v-model="modelOther2.hardness2">  <input class="OtherData" v-model="modelOther2.hardness3"></td>
                </tr>
                <tr>
                    <td rowspan="7">回火</td>
                    <td>装炉</td>
                    <td>炉温 ≤ 300℃</td>
                    <td>实际炉温：{{temperature[3]}}℃<button class="updateBtn" @click="updateBtn(3,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="3">加热</td>
                    <td rowspan="3">回火加热温度700℃左右<br>（注意：回火温度应参照淬火硬度和<br>图纸要求的调质硬度做适当调整）</td>
                    <td>起始时间：{{time[4]}} 净时间<button class="updateBtn" @click="updateBtn(4,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[5]}}<button class="updateBtn" @click="updateBtn(5,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>实际温度：  {{temperature[4]}}℃<button class="updateBtn" @click="updateBtn(4,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="2">保温</td>
                    <td rowspan="2">约2 - 3小时</td>
                    <td>起始时间：{{time[6]}} 净时间<button class="updateBtn" @click="updateBtn(6,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[7]}}<button class="updateBtn" @click="updateBtn(7,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>冷却</td>
                    <td>空冷</td>
                    <td>注意：工件放置处应干燥无水迹</td>
                </tr>
                <tr>
                    <td colspan="5">&nbsp;</td>
                </tr>
            </table>

            <table class="craftsmanshipTableHeader" v-if="model4">
                <tr>
                    <td>序号</td>
                    <td>高频淬火材料</td>
                    <td>类似材料品牌</td>
                    <td>淬火温度℃</td>
                    <td>&nbsp;</td>
                    <td>序号</td>
                    <td>高频淬火材料</td>
                    <td>类似材料品牌</td>
                    <td>淬火温度℃</td>
               </tr>
                <tr>
                    <td>1</td>
                    <td>中碳钢</td>
                    <td>35#、45#</td>
                    <td>880 - 920</td>
                    <td>&nbsp;</td>
                    <td>4</td>
                    <td>表面渗碳钢</td>
                    <td>15#钢</td>
                    <td>840 - 860</td>
                </tr>
                <tr>
                    <td>2</td>
                    <td>高碳钢</td>
                    <td>T8、T10、T12、65Mn、Gr15</td>
                    <td>850 - 890</td>
                    <td>&nbsp;</td>
                    <td>5</td>
                    <td>表面渗碳（低合金）钢</td>
                    <td>20Cr等</td>
                    <td>860 - 900</td>
                </tr>
                <tr>
                    <td>3</td>
                    <td>低合金钢</td>
                    <td>40Gr等</td>
                    <td>880 - 920</td>
                    <td>&nbsp;</td>
                    <td>6</td>
                    <td></td>
                    <td></td>
                    <td></td>
                </tr>
            </table>
            <table class="craftsmanshipTableHeader"  v-if="model4">
                <tr>
                    <td>工序</td>
                    <td>子工序</td>
                    <td>要求</td>
                    <td>记录</td>
                    <td>工艺路线</td>
                </tr>
                <tr>
                    <td rowspan="6">淬火</td>
                    <td rowspan="2">加热</td>
                    <td>加热温度（参照规程规定）</td>
                    <td>实际炉温：{{temperature[0]}}℃<button class="updateBtn2" @click="updateBtn(0,'temperature')">更新</button></td>
                    <td rowspan="13"></td>
                </tr>
                <tr>
                    <td>加热时间：<input class="OtherData" v-model="modelOther4.heating">秒</td>
                    <td>参照规程中的规定：温度用远红外线枪检测</td>
                </tr>
                <tr>
                    <td rowspan="3">冷却</td>
                    <td>在浓度为1.25%左右PAG里</td>
                    <td>实际PAG浓度：<input class="OtherData" v-model="modelOther4.PAGStrength">%</td>
                </tr>
                <tr>
                    <td>在“快速淬火油”里</td>
                    <td>实际油品质：<input class="OtherData" v-model="modelOther4.Oilquality"></td>
                </tr>
                <tr>
                    <td>在浓度为10%左右的盐水里</td>
                    <td>实际盐水浓度：<input class="OtherData" v-model="modelOther4.BrineStrength">%</td>
                </tr>
                <tr>
                    <td>硬度抽查</td>
                    <td>≥HRC50</td>
                    <td><input class="OtherData" v-model="modelOther4.hardness1">  <input class="OtherData" v-model="modelOther4.hardness2">  <input class="OtherData" v-model="modelOther4.hardness3"></td>
                </tr>
                <tr>
                    <td rowspan="6">回火</td>
                    <td>装炉</td>
                    <td>炉温≤100℃</td>
                    <td>实际炉温：{{temperature[1]}}℃<button class="updateBtn2" @click="updateBtn(1,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td rowspan="3">加热</td>
                    <td rowspan="3">（注意：回火温度应参照淬火硬度和<br>图纸要求的淬火硬度做适当调整）</td>
                    <td>起始时间：{{time[0]}} 净时间<button class="updateBtn2" @click="updateBtn(0,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>结束时间：{{time[1]}}<button class="updateBtn2" @click="updateBtn(1,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>实际温度：{{temperature[2]}}℃<button class="updateBtn2" @click="updateBtn(2,'temperature')">更新</button></td>
                </tr>
                <tr>
                    <td>保温</td>
                    <td>约1 - 2小时</td>
                    <td>起始时间：{{time[2]}} 净时间<button class="updateBtn2" @click="updateBtn(2,'time')">更新</button></td>
                </tr>
                <tr>
                    <td>冷却</td>
                    <td>空冷  注意：工件放置处应干燥无水迹</td>
                    <td>结束时间：{{time[3]}}<button class="updateBtn2" @click="updateBtn(3,'time')">更新</button></td>
                </tr>
                <tr>
                    <td colspan="2">高频淬火深度抽查</td>
                    <td>一般在0.5 - 1.5mm左右为宜</td>
                    <td>实际深度约：<input class="OtherData" v-model="modelOther4.depth1">  <input class="OtherData" v-model="modelOther4.depth2"></td>
                </tr>
            </table>
            
            <table class="craftsmanshipTableHeader">
                <tr>
                    <td colspan="11"><h1>“自检”调质处理后硬度记录（不足10件按实数检验）&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;技术要求：</h1></td>
                </tr>
                <tr>
                    <td>序号</td>
                    <td>1</td>
                    <td>2</td>
                    <td>3</td>
                    <td>4</td>
                    <td>5</td>
                    <td>6</td>
                    <td>7</td>
                    <td>8</td>
                    <td>9</td>
                    <td>10</td>
                </tr>
                <tr>
                    <td>是否合格</td>
                    <td>
                        <el-select v-model="selectData.value" placeholder="" class="selectbox">
                            <el-option
                            v-for="item in selectoptions"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>                       
                    </td>
                    <td>
                        <el-select v-model="selectData.value1" placeholder="" class="selectbox">
                            <el-option
                            v-for="item in selectoptions"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>                         
                    </td>
                    <td>
                        <el-select v-model="selectData.value2" placeholder="" class="selectbox">
                            <el-option
                            v-for="item in selectoptions"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>                         
                    </td>
                    <td>
                        <el-select v-model="selectData.value3" placeholder="" class="selectbox">
                            <el-option
                            v-for="item in selectoptions"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>                         
                    </td>
                    <td>
                        <el-select v-model="selectData.value4" placeholder="" class="selectbox">
                            <el-option
                            v-for="item in selectoptions"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>                         
                    </td>
                    <td>
                        <el-select v-model="selectData.value5" placeholder="" class="selectbox">
                            <el-option
                            v-for="item in selectoptions"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>                         
                    </td>
                    <td>
                        <el-select v-model="selectData.value6" placeholder="" class="selectbox">
                            <el-option
                            v-for="item in selectoptions"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>                         
                    </td>
                    <td>
                        <el-select v-model="selectData.value7" placeholder="" class="selectbox">
                            <el-option
                            v-for="item in selectoptions"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>                         
                    </td>
                    <td>
                        <el-select v-model="selectData.value8" placeholder="" class="selectbox">
                            <el-option
                            v-for="item in selectoptions"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>                         
                    </td>
                    <td>
                        <el-select v-model="selectData.value9" placeholder="" class="selectbox">
                            <el-option
                            v-for="item in selectoptions"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>                         
                    </td>
                </tr>
                <tr>
                    <td colspan="2">热处理人员签名</td>
                    <td colspan="6"><input type="text" v-model="selectData.Autograph"></td>
                    <td colspan="2">自检时间</td>
                    <td><input style="width:50%" v-model="selectData.Year">年<br/><input style="width:30%" v-model="selectData.Month">月<br/><input style="width:30%" v-model="selectData.Day">日</td>
                </tr>
                <tr>
                    <td rowspan="2" colspan="2">热处理最终结论</td>
                    <td colspan="3">合格</td>
                    <td colspan="3">返工</td>
                    <td colspan="3">报废</td>
                </tr>
                <tr>
                    <td colspan="3"><input class="OtherData" v-model="selectData.Qualified">件</td>
                    <td colspan="3"><input class="OtherData" v-model="selectData.Rework">件</td>
                    <td colspan="3"><input class="OtherData" v-model="selectData.Scrap">件</td>
                </tr>
                <tr>
                    <td>检验员抽检硬度</td>
                    <td colspan="10">1:HB<input class="HB" v-model="selectData.HB1"> &nbsp;&nbsp;2:HB<input class="HB" v-model="selectData.HB2"> &nbsp;&nbsp;3:HB<input class="HB" v-model="selectData.HB3"> &nbsp;&nbsp;4:HB<input class="HB" v-model="selectData.HB4"> &nbsp;&nbsp;5:HB<input class="HB" v-model="selectData.HB5"> &nbsp;&nbsp;</td>
                </tr>
                <tr>
                    <td>是否合格</td>
                    <td>
                        <el-select v-model="selectData.value10" placeholder="" class="selectbox">
                            <el-option
                            v-for="item in selectoptions"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                            </el-option>
                        </el-select>                        
                    </td>
                    <td>检验员签名</td>
                    <td colspan="2"><input type="text" v-model="selectData.Inspector"></td>
                    <td>车间责任人</td>
                    <td colspan="2"><input type="text" v-model="selectData.Conscientious"></td>
                    <td>审核</td>
                    <td colspan="2"><input type="text" v-model="selectData.Examine"></td>
                </tr>
                <tr>
                    <td colspan="8"></td>
                    <td>日期</td>
                    <td colspan="2">{{selectData.date}}</td>
                </tr>
            </table>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogcraftsmanshipVisible = false">取 消</el-button>
                <el-button type="primary" @click="craftsmanshipSaveData">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>

<script>
import axios from "axios"
export default {
    name: "HeattreatmentDialog",
    props: {

    },
    data() {
        return {
            selectedTreeNode : {//左边树选中的节点（表类型，对应的id）
                tableFlag : "",
                relateId : ""
            },
            model1:true,
            model2:false,
            model3:false,
            model4:false,
            model5:false,
            model6:false,
            dialogcraftsmanshipVisible: false,//制造工艺dialog
            craftsmanshipTableHeader: {
                "contactId" : "",//保存记录后返回的id
                "productName": "",
                "ownPartName": "",
                "partsName": "",
                "productDrawingNumber": "",
                "ownPartDrawingNumber": "",
                "partsDrawingNumber": ""
            },
            options: [{
                value: '45钢（盐水10%）调质热处理报告',
                label: '45钢（盐水10%）调质热处理报告'
                }, {
                value: '2Cr13钢调质热处理（空冷淬火）报告',
                label: '2Cr13钢调质热处理（空冷淬火）报告'
                }, {
                value: '42CrMo钢调质热处理（1.25%PAG淬火）报告',
                label: '42CrMo钢调质热处理（1.25%PAG淬火）报告'
                }, {
                value: '高频淬火 --- 热处理报告',
                label: '高频淬火 --- 热处理报告'
                }, {
                value: '40CrNiMo钢调质热处理（1.25%PAG淬火）报告',
                label: '40CrNiMo钢调质热处理（1.25%PAG淬火）报告'
                }, {
                value: '40Cr钢调质热处理（1.25%PAG淬火）报告',
                label: '40Cr钢调质热处理（1.25%PAG淬火）报告'
                }],
            temperature:['_______ ','_______ ','_______ ','_______ ','_______ '],
            time:['_______ ','_______ ','_______ ','_______ ','_______ ','_______ ','_______ ','_______ '],
            modelOther1:{
                "BrineStrength" : "",
                "SaltwaterTemperature" : "",
                "hardness1" : "",
                "hardness2" : "",
                "hardness3" : "",
            },
            modelOther2:{
                "airtemperature" : "",
                "Actual" : "",
                "hardness1" : "",
                "hardness2" : "",
                "hardness3" : "",
            },
            modelOther3:{
                "PAGStrength" : "",
                "PAGTemperature" : "",
                "PAGQuality" : "",
                "hardness1" : "",
                "hardness2" : "",
                "hardness3" : "",
            },
            modelOther4:{
                "heating" : "",
                "PAGStrength" : "",
                "Oilquality" : "",
                "BrineStrength" : "",
                "hardness1" : "",
                "hardness2" : "",
                "hardness3" : "",
                "depth1" : "",
                "depth2" : "",
            },
            modelOther5:{
                "PAGStrength" : "",
                "PAGTemperature" : "",
                "PAGQuality" : "",
                "hardness1" : "",
                "hardness2" : "",
                "hardness3" : "",
            },
            modelOther6:{
                "PAGStrength" : "",
                "PAGTemperature" : "",
                "PAGQuality" : "",
                "hardness1" : "",
                "hardness2" : "",
                "hardness3" : "",
            },                                                
            selectData:{
                "Year" : "",
                "Month" : "",
                "Day" : "",
                "date" : "____年__月__日",
                "value" : "",
                "value1" : "",
                "value2" : "",
                "value3" : "",
                "value4" : "",
                "value5" : "",
                "value6" : "",
                "value7" : "",
                "value8" : "",
                "value9" : "",
                "value10" : "",
                "Qualified" : "",
                "Rework" : "",
                "Scrap" : "",
                "HB1" : "",
                "HB2" : "",
                "HB3" : "",
                "HB4" : "",
                "HB5" : "",
                "Inspector" : "",
                "Conscientious" : "",
                "Examine" : "",
            },
            selectoptions:[{
                value: '',
                label: ''
            },{
                value: 'yes',
                label: '是'
            },{
                value: 'no',
                label: '否'
            }
            ],
            value:"45钢（盐水10%）调质热处理报告"
        }
    },
    methods: {
        //修改下拉选项选择模板
        model(value){
            resetInputCraftsmanship()
            this.temperature=['_______ ','_______ ','_______ ','_______ ','_______ ']
            this.time=['_______ ','_______ ','_______ ','_______ ','_______ ','_______ ','_______ ','_______ ']
            switch(value){
                case "45钢（盐水10%）调质热处理报告":
                    this.model1=true
                    this.model2=false
                    this.model3=false
                    this.model4=false
                    this.model5=false
                    this.model6=false
                    break;
                case "2Cr13钢调质热处理（空冷淬火）报告":
                    this.model1=false
                    this.model2=true
                    this.model3=false
                    this.model4=false
                    this.model5=false
                    this.model6=false
                    break;
                case "42CrMo钢调质热处理（1.25%PAG淬火）报告":
                    this.model1=false
                    this.model2=false
                    this.model3=true
                    this.model4=false
                    this.model5=false
                    this.model6=false
                    break;
                case "高频淬火 --- 热处理报告":
                    this.model1=false
                    this.model2=false
                    this.model3=false 
                    this.model4=true
                    this.model5=false
                    this.model6=false
                    break;
                case "40CrNiMo钢调质热处理（1.25%PAG淬火）报告":
                    this.model1=false
                    this.model2=false
                    this.model3=false 
                    this.model4=false
                    this.model5=true
                    this.model6=false
                    break;
                case "40Cr钢调质热处理（1.25%PAG淬火）报告":
                    this.model1=false
                    this.model2=false
                    this.model3=false 
                    this.model4=false
                    this.model5=false
                    this.model6=true
                    break;
            }
        },

        //重置焊接模态框的输入框
        resetInputCraftsmanship() {            
            this.craftsmanshipTableHeader = {
                "contactId" : "",//保存记录后返回的id
                "productName": "",
                "ownPartName": "",
                "partsName": "",
                "productDrawingNumber": "",
                "ownPartDrawingNumber": "",
                "partsDrawingNumber": ""
            }
            this.temperature=['_______ ','_______ ','_______ ','_______ ','_______ ']
            this.time=['_______ ','_______ ','_______ ','_______ ','_______ ','_______ ','_______ ','_______ ']
            this.selectData={"Day" : "","Month" : "","Year" : "","date" : "____年__月__日","value" : "","value1" : "","value2" : "","value3" : "","value4" : "","value5" : "","value6" : "","value7" : "","value8" : "","value9" : "","value10" : "","Qualified" : "","Rework" : "","Scrap" : "","HB1" : "","HB2" : "","HB3" : "","HB4" : "","HB5" : "","Inspector" : "","Conscientious" : "","Examine" : "",}
            this.modelOther1={"BrineStrength" : "","SaltwaterTemperature" : "","hardness1" : "","hardness2" : "","hardness3" : "",},
            this.modelOther2={"airtemperature" : "","Actual" : "","hardness1" : "","hardness2" : "","hardness3" : "",},
            this.modelOther3={"PAGStrength" : "","PAGTemperature" : "","PAGQuality" : "","hardness1" : "","hardness2" : "","hardness3" : "",},
            this.modelOther4={"heating" : "","PAGStrength" : "","Oilquality" : "","BrineStrength" : "","hardness1" : "","hardness2" : "","hardness3" : "","depth1" : "","depth2" : "",},
            this.modelOther5={"PAGStrength" : "","PAGTemperature" : "","PAGQuality" : "","hardness1" : "","hardness2" : "","hardness3" : "",},
            this.modelOther6={"PAGStrength" : "","PAGTemperature" : "","PAGQuality" : "","hardness1" : "","hardness2" : "","hardness3" : "",}            
            },
        //打开新建模态框
        openHeattreatmentDialog(selectedTreeNode){          
            this.selectedTreeNode = selectedTreeNode
            //  console.log(selectedTreeNode)
            var relateId=selectedTreeNode.relateId;
            let fd = new FormData()
            fd.append('flag','heating')
            fd.append('relateId',relateId)
            axios.post(`${this.baseURL}/basicdata/getTableHead.php`,fd)
            .then((res) =>{
                // console.log(res.data)
                this.craftsmanshipTableHeader.partsName=res.data.pnumber;
                this.craftsmanshipTableHeader.productName=res.data.procode+res.data.proname;
            })
            .catch(function (error){
                console.log(error)
            })
            this.dialogcraftsmanshipVisible = true
            if(this.craftsmanshipTableHeader.contactId){
                this.resetInputCraftsmanship()
            } 
        },
        //查看及编辑焊接信息
        Handlealter(contactId,selectedTreeNode){
            this.selectedTreeNode = selectedTreeNode
            axios.get(`${this.baseURL}/basicdata/heattreatment.php?flag=getHeattreatmentInfoData&contactID=${contactId}`)
            .then((response) => {   
                this.dialogcraftsmanshipVisible = true
                var temperature_arr=response.data.temperature.split('|');
                var popped = temperature_arr.pop();
                this.temperature=temperature_arr;
                var time_arr=response.data.time.split('|');
                var popped2 = time_arr.pop();
                this.time=time_arr;
                // console.log(temperature_arr.length)
                if(response.data.state == "success"){
                    this.craftsmanshipTableHeader = response.data.data.craftsmanshipTableHeader
                    this.value = response.data.data.value
                    switch(this.value){
                        case("45钢（盐水10%）调质热处理报告"):
                            this.modelOther1 = JSON.parse(response.data.otherData);
                            break;
                        case("2Cr13钢调质热处理（空冷淬火）报告"):
                            this.modelOther2 = JSON.parse(response.data.otherData);
                            break;
                        case("42CrMo钢调质热处理（1.25%PAG淬火）报告"):
                            this.modelOther3 = JSON.parse(response.data.otherData);
                            break;
                        case("高频淬火 --- 热处理报告"):
                            this.modelOther4 = JSON.parse(response.data.otherData);
                            break;
                        case("40CrNiMo钢调质热处理（1.25%PAG淬火）报告"):
                            this.modelOther5 = JSON.parse(response.data.otherData);
                            break;
                        case("40Cr钢调质热处理（1.25%PAG淬火）报告"):
                            this.modelOther6 = JSON.parse(response.data.otherData);
                            break;                                                                                                                       
                    }
                    this.selectData = JSON.parse(response.data.selectvalue);
                }
            })
            .catch(function(error){
                console.log(error)
            })          
        },
        //保存表格信息
        craftsmanshipSaveData(){
            //赋值当前时间
            var now = new Date(); //当前日期
            var nowDay = now.getDate(); //当前日
            var nowMonth = (now.getMonth()+1) < 10?'0'+ (now.getMonth()+1):now.getMonth()+1; //当前月
            var nowYear = now.getFullYear(); //当前年
            this.selectData.date = nowYear+'年'+nowMonth+'月'+nowDay+'日'            
            let fd = new FormData()
            fd.append("flag","heattreatmentDataOne")
                switch(this.value){
                    case("45钢（盐水10%）调质热处理报告"):
                        fd.append("otherData",JSON.stringify(this.modelOther1));//其他数据(下面也是)
                        break;
                    case("2Cr13钢调质热处理（空冷淬火）报告"):
                        fd.append("otherData",JSON.stringify(this.modelOther2));
                        break;
                    case("42CrMo钢调质热处理（1.25%PAG淬火）报告"):
                        fd.append("otherData",JSON.stringify(this.modelOther3));
                        break;
                    case("高频淬火 --- 热处理报告"):
                        fd.append("otherData",JSON.stringify(this.modelOther4));
                        break;
                    case("40CrNiMo钢调质热处理（1.25%PAG淬火）报告"):
                        fd.append("otherData",JSON.stringify(this.modelOther5));
                        break;
                    case("40Cr钢调质热处理（1.25%PAG淬火）报告"):
                        fd.append("otherData",JSON.stringify(this.modelOther6));
                        break;                                                                                                                       
                }            
                fd.append("treeId",this.selectedTreeNode.relateId)//选中树的id
                fd.append("heattreatmentTableHeader",JSON.stringify(this.craftsmanshipTableHeader))//头部信息
                fd.append("model",this.value)//模板信息
                fd.append("temperature",JSON.stringify(this.temperature))//获取温度值
                fd.append("time",JSON.stringify(this.time))//获取时间值
                fd.append("selectvalue",JSON.stringify(this.selectData))//获取select值
                axios.post(`${this.baseURL}/basicdata/heattreatment.php`,fd)
                    .then((response) => {        
                        console.log(response)                
                        if(response.data.state == "success"){
                            this.$message({
                                showClose: true,
                                message: '新增成功',
                                type: 'success'
                            })                            
                        }else{
                            this.$message({
                                showClose: true,
                                message: '新增失败',
                                type: 'error'
                            })
                        } 
                        this.$emit("refreshTable",this.selectedTreeNode)
                        this.dialogcraftsmanshipVisible = false                                           
                    })
                    .catch(function (error){
                        console.log(error)
                    })
        },
        //点击更新按钮，获取温度与时间
        updateBtn(value,type){
            let fd = new FormData()
            fd.append("flag",type)
            if(type=='temperature'){
                axios.post(`${this.baseURL}/basicdata/components/updateTemperatureTime.php`,fd)
                    .then((response) => {
                            // console.log(response)
                            let temperature=response.data;
                            this.$set(this.temperature,value,temperature);
                            // console.log(this.temperature)
                        })
                    .catch(function (error){
                            console.log(error)
                        })
            }else if(type=='time'){
                axios.post(`${this.baseURL}/basicdata/components/updateTemperatureTime.php`,fd)
                    .then((response) => {
                            // console.log(response)
                            let time=response.data;
                            this.$set(this.time,value,time);
                            // console.log(this.time)
                        })
                    .catch(function (error){
                            console.log(error)
                        })
            }

        }
    }
}
</script>

<style scoped>
input {
    border: none;
    height: 30px;
    width: 100%;
}
  .tableOne,.tableTwo,.tableThree,.tableFour,.craftsmanshipTableHeader,.craftsmanshipTableBody_1,.craftsmanshipTableFooter{
    border-collapse: collapse;
    text-align: center;
    width: 100%;
  }
  .tableOne tr,td,th,.tableTwo tr,td,th,.tableThree tr,td,th,.tableFour tr,td,th,.craftsmanshipTableHeader tr,td,th,.craftsmanshipTableBody_1 tr,td,th,.craftsmanshipTableFooter tr,td,th{
    border: 1px solid black;
    padding: 2px;
  }
  .craftsmanshipTableBody_2{
    width: 100%;
    border-collapse: collapse;
    text-align: center;
  }
  .craftsmanshipTableBody_2 tr,td,th{
    border: 1px solid black;
    padding: 2px;
  }
  .craftsmanshipTableBody_3{
    width: 100%;
    border-collapse: collapse;
    text-align: center;
    height: 500px;
  }
  .craftsmanshipTableBody_1_img{
    width: 100%;
    border-collapse: collapse;
    text-align: center;
    height: 350px;
  }
  .weldingsequence{
    height: 25px;
    width: 10%
  }
  .tableFour{
    width: 100%;
    height: 500px;
  }
  .craftsmanshipTableBody_2_img{
    width: 33%;
    height: 500px;
  }
  .serialNumber{
    width: 50px;
  }
  el-dialog__body{
      padding: 0 20px !important;
  }
  .updateBtn{
      position: absolute;
      /* right: 360px; */
      width: 40px;
  }
  .updateBtn2{
      position: absolute;
      /* right: 140px; */
      width: 40px;
  }
  .OtherData{
    width: 25%;
    border-bottom: 1px solid #000000;  
    border-top:0px;  
    border-left:0px;  
    border-right:0px; 
    text-align: center;
  }
  .selectbox{
      width: 100%;
  }
  .HB{
    width: 10%;
    border-bottom: 1px solid #000000;  
    border-top:0px;  
    border-left:0px;  
    border-right:0px; 
    text-align: center;      
  }
</style>
