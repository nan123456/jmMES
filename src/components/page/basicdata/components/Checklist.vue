<template>
    <table border="1" style="border-collapse:collapse;font-size:17px">
        <tr>
            <td colspan="37" rowspan="2">
                <h4 style="text-align: center">中山市金马科技娱乐设备股份有限公司</h4>
                <p style="text-align: end;text-align: right">YZ[Y]A-019</p>
            </td>
        </tr>
        <tr></tr>
        <tr>
            <td colspan="2" style="text-align: center;width:100px">车  间</td>
            <td style="text-align: center;width:100px">班  组</td>
            <td style="text-align: center;width:100px">  年  月</td>
            <td style="text-align: center;width:100px">设 备 编 号</td>
            <td colspan="12" rowspan="2" style="text-align: center">&nbsp;&nbsp;日&nbsp;&nbsp;&nbsp;检&nbsp;&nbsp;&nbsp;表</td>
            <td colspan="7" style="text-align: center"> 设 备 名 称</td>
            <td colspan="7" style="text-align: center"> 设 备 型 号</td>
            <td colspan="6" style="text-align: center"> 操 作 者</td>
        </tr>
        <tr>
            <td colspan="2" style="text-align: center">&nbsp;</td>
            <td style="text-align: center">&nbsp;</td>
            <td style="text-align: center">&nbsp;</td>
            <td style="text-align: center">&nbsp;</td>
            <td colspan="7" style="text-align: center">{{tabledata.name}}</td>
            <td colspan="7" style="text-align: center">&nbsp;</td>
            <td colspan="6" style="text-align: center">&nbsp;</td>
        </tr>   
        <tr>
            <td rowspan="2" style="width:30px"></td>
            <td colspan="4" rowspan="2" style="text-align: center;width:100px"> 点 检 内 容 </td>
            <td rowspan="2" style="text-align: center;width:100px">检点方法</td>
            <td colspan="31" style="text-align: center">  检  查  日  期  </td>
        </tr> 
        <tr class="td31">
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
            <td>11</td>
            <td>12</td>
            <td>13</td>
            <td>14</td>
            <td>15</td>
            <td>16</td>
            <td>17</td>
            <td>18</td>
            <td>19</td>
            <td>20</td>
            <td>21</td>
            <td>22</td>
            <td>23</td>
            <td>24</td>
            <td>25</td>
            <td>26</td>
            <td>27</td>
            <td>28</td>
            <td>29</td>
            <td>30</td>
            <td>31</td>
        </tr>  
        <tr>
            <td style="text-align: center">1</td>
            <td colspan="4">{{tabledata.content1}}</td>
            <td style="text-align: center">{{tabledata.method1}}</td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
        </tr> 
        <tr>
            <td style="text-align: center">2</td>
            <td colspan="4">{{tabledata.content2}}</td>
            <td style="text-align: center">{{tabledata.method2}}</td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
        </tr> 
        <tr>
            <td style="text-align: center">3</td>
            <td colspan="4">{{tabledata.content3}}</td>
            <td style="text-align: center">{{tabledata.method3}}</td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
        </tr>
        <tr>
            <td style="text-align: center">4</td>
            <td colspan="4">{{tabledata.content4}}</td>
            <td style="text-align: center">{{tabledata.method4}}</td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
        </tr>
        <tr>
            <td style="text-align: center">5</td>
            <td colspan="4">{{tabledata.content5}}</td>
           <td style="text-align: center">{{tabledata.method5}}</td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
        </tr>
        <tr>
            <td style="text-align: center">6</td>
            <td colspan="4">{{tabledata.content6}}</td>
            <td style="text-align: center">{{tabledata.method6}}</td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
        </tr>
        <tr>
            <td style="text-align: center">7</td>
            <td colspan="4">{{tabledata.content7}}</td>
            <td style="text-align: center">{{tabledata.method7}}</td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
        </tr>
        <tr>
            <td style="text-align: center">8</td>
            <td colspan="4">{{tabledata.content8}}</td>
            <td style="text-align: center">{{tabledata.method8}}</td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
        </tr> 
        <tr>
            <td style="text-align: center">9</td>
            <td colspan="4">{{tabledata.content9}}</td>
            <td style="text-align: center">{{tabledata.method9}}</td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
        </tr> 
        <tr>
            <td style="text-align: center">10</td>
            <td colspan="4">{{tabledata.content10}}</td>
            <td style="text-align: center">{{tabledata.method10}}</td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
        </tr>
        <tr>
            <td style="text-align: center">11</td>
            <td colspan="4">{{tabledata.content11}}</td>
            <td style="text-align: center">{{tabledata.method11}}</td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
            <td style="text-align: center"></td>
        </tr>
        <tr>
            <td colspan="6" style="text-align: center">开  动  台  时 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;全月<input type="text" style="width:25px;border: none;font-size:17px">小时</td>
            <td colspan="31" rowspan="2" style="text-align: center">注：每天将检点情况，按符号列入格内，良好"√"尚可"△"待修"×"已修"○"</td>
        </tr>
        <tr>
            <td colspan="2" rowspan="4" style="text-align: center">停开<br>台时</td>
            <td style="text-align: center">故障停台</td>
            <td colspan="3" style="text-align: right">全月<input type="text" style="width:25px;border: none;font-size:17px">小时</td>
        </tr>
        <tr>
            <td style="text-align: center">其他原因</td>
            <td colspan="3" style="text-align: right">（待料、停电、调休、无任务）<br>全月<input type="text" style="width:25px;border: none;font-size:17px">小时</td>
            <td colspan="5" rowspan="5" style="text-align: center">设备状况与修理意见</td>
            <td colspan="26" rowspan="5"></td>
        </tr>
        <tr>
            <td style="text-align: center">保    养</td>
            <td colspan="3" style="text-align: right">全月<input type="text" style="width:25px;border: none;font-size:17px">小时</td>
        </tr> 
        <tr>
            <td style="text-align: center">计划小修</td>
            <td colspan="3" style="text-align: right">全月<input type="text" style="width:25px;border: none;font-size:17px">小时</td>
        </tr> 
        <tr>
            <td colspan="2" rowspan="2" style="text-align: center">区域巡检</td>
            <td style="text-align: center">维修钳工</td>
            <td colspan="3" >（签字）</td>
        </tr>       
        <tr>
            <td style="text-align: center">维修电工</td>
            <td colspan="3">（签字）</td>
        </tr>                           
    </table>
</template>
<script>
export default {
    name:"Checklist",
    data(){
        return {
            tabledata:{
                "name":"",
                "content1":"",
                "method1":"",
                "content2":"",
                "method2":"",
                "content3":"",
                "method3":"",
                "content4":"",
                "method4":"",
                "content5":"",
                "method5":"",
                "content6":"",
                "method6":"",
                "content7":"",
                "method7":"",
                "content8":"",
                "method8":"",
                "content9":"",
                "method9":"",
                "content10":"",
                "method10":"",
                "content11":"",
                "method11":""
                }
        }
    }
}
</script>
<style scoped>
    .td31>td{
        width: 22px;
        height: 22px;
        text-align: center;
    }
</style>