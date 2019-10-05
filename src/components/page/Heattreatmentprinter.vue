<template>
    <div class="main">
        <table class="tableHeader">
            <tr>
                <td>产品热处理工艺技术要求及检验记录表</td>
            </tr>
        </table>
        <table class="tableHeader">
            <tr>
                <td>产品型号名称</td>
                <td>{{craftsmanshipTableHeader.productName}}</td>
                <td >零件名称</td>
                <td>{{craftsmanshipTableHeader.ownPartName}}</td>
                <td>工单号</td>
                <td>{{craftsmanshipTableHeader.partsName}}</td>
            </tr>
            <tr>
                <td>零件图号</td>
                <td>{{craftsmanshipTableHeader.productDrawingNumber}}</td>
                <td>数量</td>
                <td>{{craftsmanshipTableHeader.ownPartDrawingNumber}}</td>
                <td>炉号</td>
                <td>{{craftsmanshipTableHeader.partsDrawingNumber}}</td>
            </tr>
        </table>
        <table class="tableHeader">
            <tr>
                <td><h2>操&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;作&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;规&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;程&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{selectData.date}}</h2></td>
            </tr>                            
        </table>
        <table class="tableFirstModel" v-if="model==1">
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
                <td>炉温≤450℃</td>
                <td>实际炉温：{{temperature[0]}}℃</td>
            </tr>
            <tr>
                <td rowspan="3">加热</td>
                <td rowspan="2">请较准确地记录：加热升<br>温到淬火温度的时间</td>
                <td>起始时间：{{time[0]}}净时间</td>
                <td rowspan="17"></td>
            </tr>
            <tr>
                <td>结束时间：{{time[1]}}</td>
            </tr>
            <tr>
                <td>加热温度850℃ ± 10°</td>
                <td>实际温度：{{temperature[1]}}℃</td>
            </tr>
            <tr>
                <td rowspan="2">保温</td>
                <td rowspan="2">加热时间 ≈ aKD<br>(aKD计算方法参照：热处理规程中说明)</td>
                <td>起始时间：{{time[2]}}</td>
            </tr>
            <tr>
                <td>结束时间：{{time[3]}}</td>
            </tr>
            <tr>
                <td rowspan="2">冷却</td>
                <td>盐水浓度为5% - 10%</td>
                <td>实际盐水浓度：{{modelOther1.BrineStrength}}%</td>
            </tr>
            <tr>
                <td>盐水温度在淬火过程中 ＜ 50℃</td>
                <td>实际盐水温度：{{modelOther1.SaltwaterTemperature}}℃</td>
            </tr>
            <tr>
                <td>硬度抽查</td>
                <td>≥HRC43</td>
                <td>
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther1.hardness1}}
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther1.hardness2}}
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther1.hardness3}}
                </td>
            </tr>
            <tr>
                <td rowspan="7">回火</td>
                <td>装炉</td>
                <td>炉温 ≤ 300℃</td>
                <td>实际炉温：{{temperature[2]}}℃</td>
            </tr>
            <tr>
                <td rowspan="3">加热</td>
                <td rowspan="3">回火加热温度560℃左右<br>（注意：回火温度应参照淬火硬度<br>和图纸要求的调质硬度做适当调整） </td>
                <td>起始时间：{{time[4]}}</td>
            </tr>
            <tr>
                <td>结束时间：{{time[5]}}</td>
            </tr>
            <tr>
                <td>实际温度：{{temperature[3]}}℃</td>
            </tr>
            <tr>
                <td rowspan="2">保温</td>
                <td rowspan="2">约2 - 3小时</td>
                <td>起始时间：{{time[6]}}</td>
            </tr>
            <tr>
                <td>结束时间：{{time[7]}}</td>
            </tr>
            <tr>
                <td>冷却</td>
                <td>空冷</td>
                <td>注意：工件放置处应干燥无水迹</td>
            </tr>            
        </table>
        <table class="tableFirstModel" v-if="model==2">
            <tr>
                <td>工序</td>
                <td>子工序</td>
                <td>要求</td>
                <td>记录</td>
                <td rowspan="2"  style="width:30%;">工艺曲线</td>
            </tr>
            <tr>
                <td rowspan="9">淬火</td>
                <td>装炉</td>
                <td>高合金钢：炉温≤300℃</td>
                <td>实际炉温：{{temperature[0]}}℃</td>
            </tr>
            <tr>
                <td rowspan="3">加热</td>
                <td rowspan="2">请较准确地记录：加热<br>升温到淬火温度的时间</td>
                <td>起始时间： {{time[0]}}净时间</td>
                <td rowspan="17"></td>
            </tr>
            <tr>
                <td>结束时间：{{time[1]}}</td>
            </tr>
            <tr>
                <td>加热温度970℃ ± 10°</td>
                <td>实际温度： {{temperature[1]}}℃</td>
            </tr>
            <tr>
                <td rowspan="2">保温</td>
                <td rowspan="2">加热时间 ≈ aKD<br>(aKD计算方法参照：热处理规程中说明)</td>
                <td>起始时间： {{time[2]}} 净时间</td>
            </tr>
            <tr>
                <td>结束时间：{{time[3]}}</td>
            </tr>
            <tr>
                <td rowspan="2">冷却</td>
                <td>空气</td>
                <td>实际空气温度： {{modelOther2.airtemperature}}℃</td>
            </tr>
            <tr>
                <td>冷却环境干燥无水迹等</td>
                <td>实际条件：{{modelOther2.Actual}}</td>
            </tr>
            <tr>
                <td>硬度抽查</td>
                <td>≥HRC38</td>
                <td>
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther2.hardness1}}
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther2.hardness2}}
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther2.hardness3}}
                </td>
            </tr>
            <tr>
                <td rowspan="7">回火</td>
                <td>装炉</td>
                <td>炉温 ≤ 300℃</td>
                <td>实际炉温：{{temperature[3]}}℃</td>
            </tr>
            <tr>
                <td rowspan="3">加热</td>
                <td rowspan="3">回火加热温度700℃左右<br>（注意：回火温度应参照淬火硬度和<br>图纸要求的调质硬度做适当调整）</td>
                <td>起始时间： {{time[4]}}净时间</td>
            </tr>
            <tr>
                <td>结束时间：{{time[5]}}</td>
            </tr>
            <tr>
                <td>实际温度： {{temperature[4]}}℃</td>
            </tr>
            <tr>
                <td rowspan="2">保温</td>
                <td rowspan="2">约2 - 3小时</td>
                <td>起始时间： {{time[6]}}净时间</td>
            </tr>
            <tr>
                <td>结束时间：{{time[7]}}</td>
            </tr>
            <tr>
                <td>冷却</td>
                <td>空冷</td>
                <td>注意：工件放置处应干燥无水迹</td>
            </tr>
        </table>
        <table class="tableFirstModel" v-if="model==3">
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
                <td>实际炉温：{{temperature[0]}}℃</td>
            </tr>
            <tr>
                <td rowspan="3">加热</td>
                <td rowspan="2">请较准确地记录：加热<br>升温到淬火温度的时间</td>
                <td>起始时间： {{time[0]}}净时间</td>
                <td rowspan="15"></td>
            </tr>
            <tr>
                <td>结束时间：{{time[1]}}</td>
            </tr>
            <tr>
                <td>加热温度870℃ ± 10°</td>
                <td>实际温度：  {{temperature[1]}}℃</td>
            </tr>
            <tr>
                <td rowspan="2">保温</td>
                <td rowspan="2">加热时间 ≈ aKD<br>(aKD计算方法参照：热处理规程中说明)</td>
                <td>起始时间： {{time[2]}}净时间</td>
            </tr>
            <tr>
                <td>结束时间：{{time[3]}}</td>
            </tr>
            <tr>
                <td rowspan="2">冷却</td>
                <td>PAG浓度为1.25%左右</td>
                <td>实际PAG浓度：{{modelOther3.PAGStrength}}%</td>
            </tr>
            <tr>
                <td>PAG温度在淬火过程中 ＜ 40℃<br>在1.25%PAG冷至液温</td>
                <td>实际PAG温度 {{modelOther3.PAGTemperature}}℃<br>PAG溶液品质{{modelOther3.PAGQuality}} </td>
            </tr>
            <tr>
                <td>硬度抽查</td>
                <td>≥HRC45</td>
                <td>
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther3.hardness1}}
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther3.hardness2}}
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther3.hardness3}}
                </td>
            </tr>
            <tr>
                <td rowspan="7">回火</td>
                <td>装炉</td>
                <td>炉温 ≤ 300℃</td>
                <td>实际炉温：{{temperature[2]}}℃</td>
            </tr>
            <tr>
                <td rowspan="3">加热</td>
                <td rowspan="3">回火加热温度560℃左右<br>（注意：回火温度应参照淬火硬度<br>和图纸要求的调质硬度做适当调整） </td>
                <td>起始时间： {{time[4]}}净时间</td>
            </tr>
            <tr>
                <td>结束时间：{{time[5]}}</td>
            </tr>
            <tr>
                <td>实际温度：{{temperature[3]}}℃</td>
            </tr>
            <tr>
                <td rowspan="2">保温</td>
                <td rowspan="2">约2 - 3小时</td>
                <td>起始时间：{{time[6]}}净时间</td>
            </tr>
            <tr>
                <td>结束时间：{{time[7]}}</td>
            </tr>
            <tr>
                <td>冷却</td>
                <td>空冷</td>
                <td>注意：工件放置处应干燥无水迹</td>
            </tr>
        </table>  
        <table class="tableFirstModel" v-if="model==4">
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
        <table class="tableFirstModel" v-if="model==4">
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
                <td>实际炉温：{{temperature[0]}}℃</td>
                <td rowspan="13"></td>
            </tr>
            <tr>
                <td>加热时间：{{modelOther4.heating}}秒</td>
                <td>参照规程中的规定：温度用远红外线枪检测</td>
            </tr>
            <tr>
                <td rowspan="3">冷却</td>
                <td>在浓度为1.25%左右PAG里</td>
                <td>实际PAG浓度：{{modelOther4.PAGStrength}}%</td>
            </tr>
            <tr>
                <td>在“快速淬火油”里</td>
                <td>实际油品质：{{modelOther4.Oilquality}}</td>
            </tr>
            <tr>
                <td>在浓度为10%左右的盐水里</td>
                <td>实际盐水浓度：{{modelOther4.BrineStrength}}%</td>
            </tr>
            <tr>
                <td>硬度抽查</td>
                <td>≥HRC50</td>
                <td>
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther4.hardness1}}
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther4.hardness2}}
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther4.hardness3}}
                </td>
            </tr>
            <tr>
                <td rowspan="6">回火</td>
                <td>装炉</td>
                <td>炉温≤100℃</td>
                <td>实际炉温：{{temperature[1]}}℃</td>
            </tr>
            <tr>
                <td rowspan="3">加热</td>
                <td rowspan="3">（注意：回火温度应参照淬火硬度和<br>图纸要求的淬火硬度做适当调整）</td>
                <td>起始时间：{{time[0]}} 净时间</td>
            </tr>
            <tr>
                <td>结束时间：{{time[1]}}</td>
            </tr>
            <tr>
                <td>实际温度：{{temperature[2]}}</td>
            </tr>
            <tr>
                <td>保温</td>
                <td>约1 - 2小时</td>
                <td>起始时间：{{time[2]}} 净时间</td>
            </tr>
            <tr>
                <td>冷却</td>
                <td>空冷  注意：工件放置处应干燥无水迹</td>
                <td>结束时间：{{time[3]}}</td>
            </tr>
            <tr>
                <td colspan="2">高频淬火深度抽查</td>
                <td>一般在0.5 - 1.5mm左右为宜</td>
                <td>实际深度约：&nbsp;&nbsp;&nbsp;&nbsp;{{modelOther4.depth1}}&nbsp;&nbsp;&nbsp;&nbsp;{{modelOther4.depth2}}</td>
            </tr>            
        </table> 
        <table class="tableFirstModel" v-if="model==5">
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
                <td>实际炉温：{{temperature[0]}}℃</td>
            </tr>
            <tr>
                <td rowspan="3">加热</td>
                <td rowspan="2">请较准确地记录：加热<br>升温到淬火温度的时间</td>
                <td>起始时间：{{time[0]}} 净时间</td>
                <td rowspan="15"></td>
            </tr>
            <tr>
                <td>结束时间：{{time[1]}}</td>
            </tr>
            <tr>
                <td>加热温度870℃ ± 10°</td>
                <td>实际温度： {{temperature[1]}} ℃</td>
            </tr>
            <tr>
                <td rowspan="2">保温</td>
                <td rowspan="2">加热时间 ≈ aKD<br>(aKD计算方法参照：热处理规程中说明)</td>
                <td>起始时间： {{time[2]}}净时间</td>
            </tr>
            <tr>
                <td>结束时间：{{time[3]}}</td>
            </tr>
            <tr>
                <td rowspan="2">冷却</td>
                <td>PAG浓度为1.25%左右</td>
                <td>实际PAG浓度：{{modelOther5.PAGStrength}} %</td>
            </tr>
            <tr>
                <td>PAG温度在淬火过程中 ＜ 40℃<br>在1.25%PAG冷至液温</td>
                <td>实际PAG温度： {{modelOther5.PAGTemperature}}℃<br>PAG溶液品质：{{modelOther2.PAGQuality}} </td>
            </tr>
            <tr>
                <td>硬度抽查</td>
                <td>≥HRC45</td>
                <td>
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther5.hardness1}}
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther5.hardness2}}
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther5.hardness3}}
                </td>
            </tr>
            <tr>
                <td rowspan="7">回火</td>
                <td>装炉</td>
                <td>炉温 ≤ 300℃</td>
                <td>实际炉温：{{temperature[2]}}℃</td>
            </tr>
            <tr>
                <td rowspan="3">加热</td>
                <td rowspan="3">回火加热温度560℃左右<br>（注意：回火温度应参照淬火硬度<br>和图纸要求的调质硬度做适当调整） </td>
                <td>起始时间： {{time[4]}}净时间</td>
            </tr>
            <tr>
                <td>结束时间：{{time[5]}}</td>
            </tr>
            <tr>
                <td>实际温度：{{temperature[3]}}℃</td>
            </tr>
            <tr>
                <td rowspan="2">保温</td>
                <td rowspan="2">约2 - 3小时</td>
                <td>起始时间：{{time[6]}}净时间</td>
            </tr>
            <tr>
                <td>结束时间：{{time[7]}}</td>
            </tr>
            <tr>
                <td>冷却</td>
                <td>空冷</td>
                <td>注意：工件放置处应干燥无水迹</td>
            </tr>
        </table>
        <table class="tableFirstModel" v-if="model==6">
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
                <td>实际炉温：{{temperature[0]}}℃</td>
            </tr>
            <tr>
                <td rowspan="3">加热</td>
                <td rowspan="2">请较准确地记录：加热<br>升温到淬火温度的时间</td>
                <td>起始时间：{{time[0]}} 净时间</td>
                <td rowspan="15"></td>
            </tr>
            <tr>
                <td>结束时间：{{time[1]}}</td>
            </tr>
            <tr>
                <td>加热温度870℃ ± 10°</td>
                <td>实际温度： {{temperature[1]}}  ℃</td>
            </tr>
            <tr>
                <td rowspan="2">保温</td>
                <td rowspan="2">加热时间 ≈ aKD<br>(aKD计算方法参照：热处理规程中说明)</td>
                <td>起始时间：{{time[2]}} 净时间</td>
            </tr>
            <tr>
                <td>结束时间：{{time[3]}}</td>
            </tr>
            <tr>
                <td rowspan="2">冷却</td>
                <td>PAG浓度为1.25%左右</td>
                <td>实际PAG浓度：{{modelOther6.PAGStrength}}%</td>
            </tr>
            <tr>
                <td>PAG温度在淬火过程中 ＜ 40℃<br>在1.25%PAG冷至液温</td>
                <td>实际PAG温度：{{modelOther6.PAGTemperature}}℃<br>PAG溶液品质：{{modelOther6.PAGQuality}}</td>
            </tr>
            <tr>
                <td>硬度抽查</td>
                <td>≥HRC45</td>
                <td>
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther6.hardness1}}  
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther6.hardness2}}  
                    &nbsp;&nbsp;&nbsp;&nbsp;{{modelOther6.hardness3}}  
                </td>
            </tr>
            <tr>
                <td rowspan="7">回火</td>
                <td>装炉</td>
                <td>炉温 ≤ 300℃</td>
                <td>实际炉温：{{temperature[2]}}℃</td>
            </tr>
            <tr>
                <td rowspan="3">加热</td>
                <td rowspan="3">回火加热温度560℃左右<br>（注意：回火温度应参照淬火硬度<br>和图纸要求的调质硬度做适当调整） </td>
                <td>起始时间：{{time[4]}} 净时间</td>
            </tr>
            <tr>
                <td>结束时间：{{time[5]}}</td>
            </tr>
            <tr>
                <td>实际温度：{{temperature[3]}}℃</td>
            </tr>
            <tr>
                <td rowspan="2">保温</td>
                <td rowspan="2">约2 - 3小时</td>
                <td>起始时间： {{time[6]}}净时间</td>
            </tr>
            <tr>
                <td>结束时间：{{time[7]}}</td>
            </tr>
            <tr>
                <td>冷却</td>
                <td>空冷</td>
                <td>注意：工件放置处应干燥无水迹</td>
            </tr>           
        </table>       
        <table class="tableHeader">
            <tr>
                <td colspan="12">“自检”调质处理后硬度记录（不足10件按实数检验）&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;技术要求：{{selectData.technical}}</td>
            </tr>
            <tr>
                <td colspan="1" class="td1">序号</td>
                <td colspan="1" class="td1">1</td>
                <td colspan="1" class="td1">2</td>
                <td colspan="1" class="td1">3</td>
                <td colspan="1" class="td1">4</td>
                <td colspan="1" class="td1">5</td>
                <td colspan="1" class="td1">6</td>
                <td colspan="1" class="td1">7</td>
                <td colspan="1" class="td1">8</td>
                <td colspan="1" class="td1">9</td>
                <td colspan="1" class="td1">10</td>
            </tr>
            <tr>
                <td colspan="1">是否合格</td>
                <td colspan="1">
                   <p v-if="selectData.value=='yes'">是</p>
                   <p v-if="selectData.value=='no'">否</p>                     
                </td>
                <td colspan="1">
                   <p v-if="selectData.value1=='yes'">是</p>
                   <p v-if="selectData.value1=='no'">否</p>                           
                </td>
                <td colspan="1">
                    <p v-if="selectData.value2=='yes'">是</p>
                    <p v-if="selectData.value2=='no'">否</p>                          
                </td>
                <td colspan="1">
                    <p v-if="selectData.value3=='yes'">是</p>
                    <p v-if="selectData.value3=='no'">否</p>                         
                </td>
                <td colspan="1">
                    <p v-if="selectData.value4=='yes'">是</p>
                    <p v-if="selectData.value4=='no'">否</p>                         
                </td>
                <td colspan="1">
                    <p v-if="selectData.value5=='yes'">是</p>
                    <p v-if="selectData.value5=='no'">否</p>                        
                </td>
                <td colspan="1">
                    <p v-if="selectData.value6=='yes'">是</p>
                    <p v-if="selectData.value6=='no'">否</p>                          
                </td>
                <td colspan="1">
                    <p v-if="selectData.value7=='yes'">是</p>
                    <p v-if="selectData.value7=='no'">否</p>                          
                </td>
                <td colspan="1">
                    <p v-if="selectData.value8=='yes'">是</p>
                    <p v-if="selectData.value8=='no'">否</p>                           
                </td>
                <td colspan="1">
                    <p v-if="selectData.value9=='yes'">是</p>
                    <p v-if="selectData.value9=='no'">否</p>                           
                </td>
            </tr>
            <tr>
                <td colspan="2">热处理人员签名</td>
                <td colspan="6">{{selectData.Autograph}}</td>
                <td colspan="1">自检时间</td>
                <td colspan="2"> {{selectData.Year}}年{{selectData.Month}}月{{selectData.Day}}日</td>
            </tr>
            <tr>
                <td rowspan="2" colspan="2">热处理最终结论</td>
                <td colspan="3">合格</td>
                <td colspan="3">返工</td>
                <td colspan="3">报废</td>
            </tr>
            <tr>
                <td colspan="3">{{selectData.Qualified}}件</td>
                <td colspan="3">{{selectData.Rework}}件</td>
                <td colspan="3">{{selectData.Scrap}}件</td>
            </tr>
            <tr>
                <td colspan="2">检验员抽检硬度</td>
                <td colspan="9">1:HB&nbsp;{{selectData.HB1}}&nbsp;&nbsp;&nbsp;&nbsp;2:HB&nbsp;{{selectData.HB2}}&nbsp;&nbsp;&nbsp;&nbsp;3:HB&nbsp;{{selectData.HB3}}&nbsp;&nbsp;&nbsp;&nbsp;4:HB&nbsp;{{selectData.HB4}}&nbsp;&nbsp;&nbsp;&nbsp;5:HB&nbsp;{{selectData.HB5}}&nbsp;&nbsp;&nbsp;&nbsp;</td>
            </tr>
            <tr>
                <td colspan="2">是否合格</td>
                <td colspan="1">
                    <p v-if="selectData.value10=='yes'">是</p>
                    <p v-if="selectData.value10=='no'">否</p>
                </td>
                <td colspan="2">检验员签名</td>
                <td colspan="1">{{selectData.Inspector}}</td>
                <td colspan="2">车间责任人</td>
                <td colspan="1">{{selectData.Conscientious}}</td>
                <td colspan="1">审核</td>
                <td colspan="1">{{selectData.Examine}}</td>
            </tr>
            <tr>
                <td colspan="8"></td>
                <td colspan="1">日期</td>
                <td colspan="2">{{selectData.date}}</td>
            </tr>            
        </table>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name:'Heattreatmentprinter',
    data(){
        return{
            // tablecraftsmanshipBodyVisible: [false,false,false,false,false,false],//模板显示选择
            model : 1 ,
            craftsmanshipTableHeader: {
                "contactId" : "",//保存记录后返回的id
                "productName": "",
                "ownPartName": "",
                "partsName": "",
                "productDrawingNumber": "",
                "ownPartDrawingNumber": "",
                "partsDrawingNumber": ""
            },
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
                "technical" : ""
            },        
        }              
    },
    created() {
        this.getInfoData()
    },
    methods: {
        getInfoData(){
            //清除默认样式
            document.getElementsByTagName("html")[0].style.overflow = "visible"
            document.getElementsByTagName("body")[0].style.overflow = "visible"
            document.getElementById("app").style.overflow = "visible"
            
            let contactId = this.$route.query.contactId//获取url的参数
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
        }
    }    
}
</script>

<style>
    .main {
        width: 100%;
        align-content: center;
    }
    table,tr {
        border-collapse: collapse;
        text-align: center;
    }
    table {
        border-collapse: collapse;
    }
    td {
        border: 1px solid black;
        border-collapse: collapse;
        padding-left: 1mm;
        padding-right: 1mm;
    }    
    /* 表格头部 */
    .tableHeader {
        width: 209mm;
        text-align: center;
        font-size: 10.5pt;
    }
    /* 模板一 */
    .tableFirstModel {
        width: 209mm;
        text-align: center;
        font-size: 10.5pt;
    }
    .td1{
        width: 17.5mm;
    }        
</style>