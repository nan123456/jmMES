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
            <td colspan="2" style="text-align: center">{{header.workshop}}</td>
            <td style="text-align: center">{{header.group}}</td>
            <td style="text-align: center">{{header.year_month}}</td>
            <td style="text-align: center">{{header.e_number}}</td>
            <td colspan="7" style="text-align: center">{{header.e_name}}</td>
            <td colspan="7" style="text-align: center">{{header.e_type}}</td>
            <td colspan="6" style="text-align: center">{{header.e_user}}</td>
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
            <td colspan="4">{{tabledata[0]["content1"]}}</td>
            <td style="text-align: center">{{tabledata[0]["method1"]}}</td>
            <td style="text-align: center">{{tabledata[0]["1"]}}</td>
            <td style="text-align: center">{{tabledata[0]["2"]}}</td>
            <td style="text-align: center">{{tabledata[0]["3"]}}</td>
            <td style="text-align: center">{{tabledata[0]["4"]}}</td>
            <td style="text-align: center">{{tabledata[0]["5"]}}</td>
            <td style="text-align: center">{{tabledata[0]["6"]}}</td>
            <td style="text-align: center">{{tabledata[0]["7"]}}</td>
            <td style="text-align: center">{{tabledata[0]["8"]}}</td>
            <td style="text-align: center">{{tabledata[0]["9"]}}</td>
            <td style="text-align: center">{{tabledata[0]["10"]}}</td>
            <td style="text-align: center">{{tabledata[0]["11"]}}</td>
            <td style="text-align: center">{{tabledata[0]["12"]}}</td>
            <td style="text-align: center">{{tabledata[0]["13"]}}</td>
            <td style="text-align: center">{{tabledata[0]["14"]}}</td>
            <td style="text-align: center">{{tabledata[0]["15"]}}</td>
            <td style="text-align: center">{{tabledata[0]["16"]}}</td>
            <td style="text-align: center">{{tabledata[0]["17"]}}</td>
            <td style="text-align: center">{{tabledata[0]["18"]}}</td>
            <td style="text-align: center">{{tabledata[0]["19"]}}</td>
            <td style="text-align: center">{{tabledata[0]["20"]}}</td>
            <td style="text-align: center">{{tabledata[0]["21"]}}</td>
            <td style="text-align: center">{{tabledata[0]["22"]}}</td>
            <td style="text-align: center">{{tabledata[0]["23"]}}</td>
            <td style="text-align: center">{{tabledata[0]["24"]}}</td>
            <td style="text-align: center">{{tabledata[0]["25"]}}</td>
            <td style="text-align: center">{{tabledata[0]["26"]}}</td>
            <td style="text-align: center">{{tabledata[0]["27"]}}</td>
            <td style="text-align: center">{{tabledata[0]["28"]}}</td>
            <td style="text-align: center">{{tabledata[0]["29"]}}</td>
            <td style="text-align: center">{{tabledata[0]["30"]}}</td>
            <td style="text-align: center">{{tabledata[0]["31"]}}</td>
        </tr> 
        <tr>
            <td style="text-align: center">2</td>
            <td colspan="4">{{tabledata[1]["content2"]}}</td>
            <td style="text-align: center">{{tabledata[1]["method2"]}}</td>
            <td style="text-align: center">{{tabledata[1]["1"]}}</td>
            <td style="text-align: center">{{tabledata[1]["2"]}}</td>
            <td style="text-align: center">{{tabledata[1]["3"]}}</td>
            <td style="text-align: center">{{tabledata[1]["4"]}}</td>
            <td style="text-align: center">{{tabledata[1]["5"]}}</td>
            <td style="text-align: center">{{tabledata[1]["6"]}}</td>
            <td style="text-align: center">{{tabledata[1]["7"]}}</td>
            <td style="text-align: center">{{tabledata[1]["8"]}}</td>
            <td style="text-align: center">{{tabledata[1]["9"]}}</td>
            <td style="text-align: center">{{tabledata[1]["10"]}}</td>
            <td style="text-align: center">{{tabledata[1]["11"]}}</td>
            <td style="text-align: center">{{tabledata[1]["12"]}}</td>
            <td style="text-align: center">{{tabledata[1]["13"]}}</td>
            <td style="text-align: center">{{tabledata[1]["14"]}}</td>
            <td style="text-align: center">{{tabledata[1]["15"]}}</td>
            <td style="text-align: center">{{tabledata[1]["16"]}}</td>
            <td style="text-align: center">{{tabledata[1]["17"]}}</td>
            <td style="text-align: center">{{tabledata[1]["18"]}}</td>
            <td style="text-align: center">{{tabledata[1]["19"]}}</td>
            <td style="text-align: center">{{tabledata[1]["20"]}}</td>
            <td style="text-align: center">{{tabledata[1]["21"]}}</td>
            <td style="text-align: center">{{tabledata[1]["22"]}}</td>
            <td style="text-align: center">{{tabledata[1]["23"]}}</td>
            <td style="text-align: center">{{tabledata[1]["24"]}}</td>
            <td style="text-align: center">{{tabledata[1]["25"]}}</td>
            <td style="text-align: center">{{tabledata[1]["26"]}}</td>
            <td style="text-align: center">{{tabledata[1]["27"]}}</td>
            <td style="text-align: center">{{tabledata[1]["28"]}}</td>
            <td style="text-align: center">{{tabledata[1]["29"]}}</td>
            <td style="text-align: center">{{tabledata[1]["30"]}}</td>
            <td style="text-align: center">{{tabledata[1]["31"]}}</td>
        </tr> 
        <tr>
            <td style="text-align: center">3</td>
            <td colspan="4">{{tabledata[2]["content3"]}}</td>
            <td style="text-align: center">{{tabledata[2]["method3"]}}</td>
            <td style="text-align: center">{{tabledata[2]["1"]}}</td>
            <td style="text-align: center">{{tabledata[2]["2"]}}</td>
            <td style="text-align: center">{{tabledata[2]["3"]}}</td>
            <td style="text-align: center">{{tabledata[2]["4"]}}</td>
            <td style="text-align: center">{{tabledata[2]["5"]}}</td>
            <td style="text-align: center">{{tabledata[2]["6"]}}</td>
            <td style="text-align: center">{{tabledata[2]["7"]}}</td>
            <td style="text-align: center">{{tabledata[2]["8"]}}</td>
            <td style="text-align: center">{{tabledata[2]["9"]}}</td>
            <td style="text-align: center">{{tabledata[2]["10"]}}</td>
            <td style="text-align: center">{{tabledata[2]["11"]}}</td>
            <td style="text-align: center">{{tabledata[2]["12"]}}</td>
            <td style="text-align: center">{{tabledata[2]["13"]}}</td>
            <td style="text-align: center">{{tabledata[2]["14"]}}</td>
            <td style="text-align: center">{{tabledata[2]["15"]}}</td>
            <td style="text-align: center">{{tabledata[2]["16"]}}</td>
            <td style="text-align: center">{{tabledata[2]["17"]}}</td>
            <td style="text-align: center">{{tabledata[2]["18"]}}</td>
            <td style="text-align: center">{{tabledata[2]["19"]}}</td>
            <td style="text-align: center">{{tabledata[2]["20"]}}</td>
            <td style="text-align: center">{{tabledata[2]["21"]}}</td>
            <td style="text-align: center">{{tabledata[2]["22"]}}</td>
            <td style="text-align: center">{{tabledata[2]["23"]}}</td>
            <td style="text-align: center">{{tabledata[2]["24"]}}</td>
            <td style="text-align: center">{{tabledata[2]["25"]}}</td>
            <td style="text-align: center">{{tabledata[2]["26"]}}</td>
            <td style="text-align: center">{{tabledata[2]["27"]}}</td>
            <td style="text-align: center">{{tabledata[2]["28"]}}</td>
            <td style="text-align: center">{{tabledata[2]["29"]}}</td>
            <td style="text-align: center">{{tabledata[2]["30"]}}</td>
            <td style="text-align: center">{{tabledata[2]["31"]}}</td>
        </tr>
        <tr>
            <td style="text-align: center">4</td>
            <td colspan="4">{{tabledata[3]["content4"]}}</td>
            <td style="text-align: center">{{tabledata[3]["method4"]}}</td>
            <td style="text-align: center">{{tabledata[3]["1"]}}</td>
            <td style="text-align: center">{{tabledata[3]["2"]}}</td>
            <td style="text-align: center">{{tabledata[3]["3"]}}</td>
            <td style="text-align: center">{{tabledata[3]["4"]}}</td>
            <td style="text-align: center">{{tabledata[3]["5"]}}</td>
            <td style="text-align: center">{{tabledata[3]["6"]}}</td>
            <td style="text-align: center">{{tabledata[3]["7"]}}</td>
            <td style="text-align: center">{{tabledata[3]["8"]}}</td>
            <td style="text-align: center">{{tabledata[3]["9"]}}</td>
            <td style="text-align: center">{{tabledata[3]["10"]}}</td>
            <td style="text-align: center">{{tabledata[3]["11"]}}</td>
            <td style="text-align: center">{{tabledata[3]["12"]}}</td>
            <td style="text-align: center">{{tabledata[3]["13"]}}</td>
            <td style="text-align: center">{{tabledata[3]["14"]}}</td>
            <td style="text-align: center">{{tabledata[3]["15"]}}</td>
            <td style="text-align: center">{{tabledata[3]["16"]}}</td>
            <td style="text-align: center">{{tabledata[3]["17"]}}</td>
            <td style="text-align: center">{{tabledata[3]["18"]}}</td>
            <td style="text-align: center">{{tabledata[3]["19"]}}</td>
            <td style="text-align: center">{{tabledata[3]["20"]}}</td>
            <td style="text-align: center">{{tabledata[3]["21"]}}</td>
            <td style="text-align: center">{{tabledata[3]["22"]}}</td>
            <td style="text-align: center">{{tabledata[3]["23"]}}</td>
            <td style="text-align: center">{{tabledata[3]["24"]}}</td>
            <td style="text-align: center">{{tabledata[3]["25"]}}</td>
            <td style="text-align: center">{{tabledata[3]["26"]}}</td>
            <td style="text-align: center">{{tabledata[3]["27"]}}</td>
            <td style="text-align: center">{{tabledata[3]["28"]}}</td>
            <td style="text-align: center">{{tabledata[3]["29"]}}</td>
            <td style="text-align: center">{{tabledata[3]["30"]}}</td>
            <td style="text-align: center">{{tabledata[3]["31"]}}</td>
        </tr>
        <tr>
            <td style="text-align: center">5</td>
            <td colspan="4">{{tabledata[4]["content5"]}}</td>
            <td style="text-align: center">{{tabledata[4]["method5"]}}</td>
            <td style="text-align: center">{{tabledata[4]["1"]}}</td>
            <td style="text-align: center">{{tabledata[4]["2"]}}</td>
            <td style="text-align: center">{{tabledata[4]["3"]}}</td>
            <td style="text-align: center">{{tabledata[4]["4"]}}</td>
            <td style="text-align: center">{{tabledata[4]["5"]}}</td>
            <td style="text-align: center">{{tabledata[4]["6"]}}</td>
            <td style="text-align: center">{{tabledata[4]["7"]}}</td>
            <td style="text-align: center">{{tabledata[4]["8"]}}</td>
            <td style="text-align: center">{{tabledata[4]["9"]}}</td>
            <td style="text-align: center">{{tabledata[4]["10"]}}</td>
            <td style="text-align: center">{{tabledata[4]["11"]}}</td>
            <td style="text-align: center">{{tabledata[4]["12"]}}</td>
            <td style="text-align: center">{{tabledata[4]["13"]}}</td>
            <td style="text-align: center">{{tabledata[4]["14"]}}</td>
            <td style="text-align: center">{{tabledata[4]["15"]}}</td>
            <td style="text-align: center">{{tabledata[4]["16"]}}</td>
            <td style="text-align: center">{{tabledata[4]["17"]}}</td>
            <td style="text-align: center">{{tabledata[4]["18"]}}</td>
            <td style="text-align: center">{{tabledata[4]["19"]}}</td>
            <td style="text-align: center">{{tabledata[4]["20"]}}</td>
            <td style="text-align: center">{{tabledata[4]["21"]}}</td>
            <td style="text-align: center">{{tabledata[4]["22"]}}</td>
            <td style="text-align: center">{{tabledata[4]["23"]}}</td>
            <td style="text-align: center">{{tabledata[4]["24"]}}</td>
            <td style="text-align: center">{{tabledata[4]["25"]}}</td>
            <td style="text-align: center">{{tabledata[4]["26"]}}</td>
            <td style="text-align: center">{{tabledata[4]["27"]}}</td>
            <td style="text-align: center">{{tabledata[4]["28"]}}</td>
            <td style="text-align: center">{{tabledata[4]["29"]}}</td>
            <td style="text-align: center">{{tabledata[4]["30"]}}</td>
            <td style="text-align: center">{{tabledata[4]["31"]}}</td>
        </tr>
        <tr>
            <td style="text-align: center">6</td>
            <td colspan="4">{{tabledata[5]["content6"]}}</td>
            <td style="text-align: center">{{tabledata[5]["method6"]}}</td>
            <td style="text-align: center">{{tabledata[5]["1"]}}</td>
            <td style="text-align: center">{{tabledata[5]["2"]}}</td>
            <td style="text-align: center">{{tabledata[5]["3"]}}</td>
            <td style="text-align: center">{{tabledata[5]["4"]}}</td>
            <td style="text-align: center">{{tabledata[5]["5"]}}</td>
            <td style="text-align: center">{{tabledata[5]["6"]}}</td>
            <td style="text-align: center">{{tabledata[5]["7"]}}</td>
            <td style="text-align: center">{{tabledata[5]["8"]}}</td>
            <td style="text-align: center">{{tabledata[5]["9"]}}</td>
            <td style="text-align: center">{{tabledata[5]["10"]}}</td>
            <td style="text-align: center">{{tabledata[5]["11"]}}</td>
            <td style="text-align: center">{{tabledata[5]["12"]}}</td>
            <td style="text-align: center">{{tabledata[5]["13"]}}</td>
            <td style="text-align: center">{{tabledata[5]["14"]}}</td>
            <td style="text-align: center">{{tabledata[5]["15"]}}</td>
            <td style="text-align: center">{{tabledata[5]["16"]}}</td>
            <td style="text-align: center">{{tabledata[5]["17"]}}</td>
            <td style="text-align: center">{{tabledata[5]["18"]}}</td>
            <td style="text-align: center">{{tabledata[5]["19"]}}</td>
            <td style="text-align: center">{{tabledata[5]["20"]}}</td>
            <td style="text-align: center">{{tabledata[5]["21"]}}</td>
            <td style="text-align: center">{{tabledata[5]["22"]}}</td>
            <td style="text-align: center">{{tabledata[5]["23"]}}</td>
            <td style="text-align: center">{{tabledata[5]["24"]}}</td>
            <td style="text-align: center">{{tabledata[5]["25"]}}</td>
            <td style="text-align: center">{{tabledata[5]["26"]}}</td>
            <td style="text-align: center">{{tabledata[5]["27"]}}</td>
            <td style="text-align: center">{{tabledata[5]["28"]}}</td>
            <td style="text-align: center">{{tabledata[5]["29"]}}</td>
            <td style="text-align: center">{{tabledata[5]["30"]}}</td>
            <td style="text-align: center">{{tabledata[5]["31"]}}</td>
        </tr>
        <tr>
            <td style="text-align: center">7</td>
            <td colspan="4">{{tabledata[6]["content7"]}}</td>
            <td style="text-align: center">{{tabledata[6]["method7"]}}</td>
            <td style="text-align: center">{{tabledata[6]["1"]}}</td>
            <td style="text-align: center">{{tabledata[6]["2"]}}</td>
            <td style="text-align: center">{{tabledata[6]["3"]}}</td>
            <td style="text-align: center">{{tabledata[6]["4"]}}</td>
            <td style="text-align: center">{{tabledata[6]["5"]}}</td>
            <td style="text-align: center">{{tabledata[6]["6"]}}</td>
            <td style="text-align: center">{{tabledata[6]["7"]}}</td>
            <td style="text-align: center">{{tabledata[6]["8"]}}</td>
            <td style="text-align: center">{{tabledata[6]["9"]}}</td>
            <td style="text-align: center">{{tabledata[6]["10"]}}</td>
            <td style="text-align: center">{{tabledata[6]["11"]}}</td>
            <td style="text-align: center">{{tabledata[6]["12"]}}</td>
            <td style="text-align: center">{{tabledata[6]["13"]}}</td>
            <td style="text-align: center">{{tabledata[6]["14"]}}</td>
            <td style="text-align: center">{{tabledata[6]["15"]}}</td>
            <td style="text-align: center">{{tabledata[6]["16"]}}</td>
            <td style="text-align: center">{{tabledata[6]["17"]}}</td>
            <td style="text-align: center">{{tabledata[6]["18"]}}</td>
            <td style="text-align: center">{{tabledata[6]["19"]}}</td>
            <td style="text-align: center">{{tabledata[6]["20"]}}</td>
            <td style="text-align: center">{{tabledata[6]["21"]}}</td>
            <td style="text-align: center">{{tabledata[6]["22"]}}</td>
            <td style="text-align: center">{{tabledata[6]["23"]}}</td>
            <td style="text-align: center">{{tabledata[6]["24"]}}</td>
            <td style="text-align: center">{{tabledata[6]["25"]}}</td>
            <td style="text-align: center">{{tabledata[6]["26"]}}</td>
            <td style="text-align: center">{{tabledata[6]["27"]}}</td>
            <td style="text-align: center">{{tabledata[6]["28"]}}</td>
            <td style="text-align: center">{{tabledata[6]["29"]}}</td>
            <td style="text-align: center">{{tabledata[6]["30"]}}</td>
            <td style="text-align: center">{{tabledata[6]["31"]}}</td>
        </tr>
        <tr>
            <td style="text-align: center">8</td>
            <td colspan="4">{{tabledata[7]["content8"]}}</td>
            <td style="text-align: center">{{tabledata[7]["method8"]}}</td>
            <td style="text-align: center">{{tabledata[7]["1"]}}</td>
            <td style="text-align: center">{{tabledata[7]["2"]}}</td>
            <td style="text-align: center">{{tabledata[7]["3"]}}</td>
            <td style="text-align: center">{{tabledata[7]["4"]}}</td>
            <td style="text-align: center">{{tabledata[7]["5"]}}</td>
            <td style="text-align: center">{{tabledata[7]["6"]}}</td>
            <td style="text-align: center">{{tabledata[7]["7"]}}</td>
            <td style="text-align: center">{{tabledata[7]["8"]}}</td>
            <td style="text-align: center">{{tabledata[7]["9"]}}</td>
            <td style="text-align: center">{{tabledata[7]["10"]}}</td>
            <td style="text-align: center">{{tabledata[7]["11"]}}</td>
            <td style="text-align: center">{{tabledata[7]["12"]}}</td>
            <td style="text-align: center">{{tabledata[7]["13"]}}</td>
            <td style="text-align: center">{{tabledata[7]["14"]}}</td>
            <td style="text-align: center">{{tabledata[7]["15"]}}</td>
            <td style="text-align: center">{{tabledata[7]["16"]}}</td>
            <td style="text-align: center">{{tabledata[7]["17"]}}</td>
            <td style="text-align: center">{{tabledata[7]["18"]}}</td>
            <td style="text-align: center">{{tabledata[7]["19"]}}</td>
            <td style="text-align: center">{{tabledata[7]["20"]}}</td>
            <td style="text-align: center">{{tabledata[7]["21"]}}</td>
            <td style="text-align: center">{{tabledata[7]["22"]}}</td>
            <td style="text-align: center">{{tabledata[7]["23"]}}</td>
            <td style="text-align: center">{{tabledata[7]["24"]}}</td>
            <td style="text-align: center">{{tabledata[7]["25"]}}</td>
            <td style="text-align: center">{{tabledata[7]["26"]}}</td>
            <td style="text-align: center">{{tabledata[7]["27"]}}</td>
            <td style="text-align: center">{{tabledata[7]["28"]}}</td>
            <td style="text-align: center">{{tabledata[7]["29"]}}</td>
            <td style="text-align: center">{{tabledata[7]["30"]}}</td>
            <td style="text-align: center">{{tabledata[7]["31"]}}</td>
        </tr> 
        <tr>
            <td style="text-align: center">9</td>
            <td colspan="4">{{tabledata[8]["content9"]}}</td>
            <td style="text-align: center">{{tabledata[8]["method9"]}}</td>
            <td style="text-align: center">{{tabledata[8]["1"]}}</td>
            <td style="text-align: center">{{tabledata[8]["2"]}}</td>
            <td style="text-align: center">{{tabledata[8]["3"]}}</td>
            <td style="text-align: center">{{tabledata[8]["4"]}}</td>
            <td style="text-align: center">{{tabledata[8]["5"]}}</td>
            <td style="text-align: center">{{tabledata[8]["6"]}}</td>
            <td style="text-align: center">{{tabledata[8]["7"]}}</td>
            <td style="text-align: center">{{tabledata[8]["8"]}}</td>
            <td style="text-align: center">{{tabledata[8]["9"]}}</td>
            <td style="text-align: center">{{tabledata[8]["10"]}}</td>
            <td style="text-align: center">{{tabledata[8]["11"]}}</td>
            <td style="text-align: center">{{tabledata[8]["12"]}}</td>
            <td style="text-align: center">{{tabledata[8]["13"]}}</td>
            <td style="text-align: center">{{tabledata[8]["14"]}}</td>
            <td style="text-align: center">{{tabledata[8]["15"]}}</td>
            <td style="text-align: center">{{tabledata[8]["16"]}}</td>
            <td style="text-align: center">{{tabledata[8]["17"]}}</td>
            <td style="text-align: center">{{tabledata[8]["18"]}}</td>
            <td style="text-align: center">{{tabledata[8]["19"]}}</td>
            <td style="text-align: center">{{tabledata[8]["20"]}}</td>
            <td style="text-align: center">{{tabledata[8]["21"]}}</td>
            <td style="text-align: center">{{tabledata[8]["22"]}}</td>
            <td style="text-align: center">{{tabledata[8]["23"]}}</td>
            <td style="text-align: center">{{tabledata[8]["24"]}}</td>
            <td style="text-align: center">{{tabledata[8]["25"]}}</td>
            <td style="text-align: center">{{tabledata[8]["26"]}}</td>
            <td style="text-align: center">{{tabledata[8]["27"]}}</td>
            <td style="text-align: center">{{tabledata[8]["28"]}}</td>
            <td style="text-align: center">{{tabledata[8]["29"]}}</td>
            <td style="text-align: center">{{tabledata[8]["30"]}}</td>
            <td style="text-align: center">{{tabledata[8]["31"]}}</td>
        </tr> 
        <tr>
            <td style="text-align: center">10</td>
            <td colspan="4">{{tabledata[9]["content10"]}}</td>
            <td style="text-align: center">{{tabledata[9]["method10"]}}</td>
            <td style="text-align: center">{{tabledata[9]["1"]}}</td>
            <td style="text-align: center">{{tabledata[9]["2"]}}</td>
            <td style="text-align: center">{{tabledata[9]["3"]}}</td>
            <td style="text-align: center">{{tabledata[9]["4"]}}</td>
            <td style="text-align: center">{{tabledata[9]["5"]}}</td>
            <td style="text-align: center">{{tabledata[9]["6"]}}</td>
            <td style="text-align: center">{{tabledata[9]["7"]}}</td>
            <td style="text-align: center">{{tabledata[9]["8"]}}</td>
            <td style="text-align: center">{{tabledata[9]["9"]}}</td>
            <td style="text-align: center">{{tabledata[9]["10"]}}</td>
            <td style="text-align: center">{{tabledata[9]["11"]}}</td>
            <td style="text-align: center">{{tabledata[9]["12"]}}</td>
            <td style="text-align: center">{{tabledata[9]["13"]}}</td>
            <td style="text-align: center">{{tabledata[9]["14"]}}</td>
            <td style="text-align: center">{{tabledata[9]["15"]}}</td>
            <td style="text-align: center">{{tabledata[9]["16"]}}</td>
            <td style="text-align: center">{{tabledata[9]["17"]}}</td>
            <td style="text-align: center">{{tabledata[9]["18"]}}</td>
            <td style="text-align: center">{{tabledata[9]["19"]}}</td>
            <td style="text-align: center">{{tabledata[9]["20"]}}</td>
            <td style="text-align: center">{{tabledata[9]["21"]}}</td>
            <td style="text-align: center">{{tabledata[9]["22"]}}</td>
            <td style="text-align: center">{{tabledata[9]["23"]}}</td>
            <td style="text-align: center">{{tabledata[9]["24"]}}</td>
            <td style="text-align: center">{{tabledata[9]["25"]}}</td>
            <td style="text-align: center">{{tabledata[9]["26"]}}</td>
            <td style="text-align: center">{{tabledata[9]["27"]}}</td>
            <td style="text-align: center">{{tabledata[9]["28"]}}</td>
            <td style="text-align: center">{{tabledata[9]["29"]}}</td>
            <td style="text-align: center">{{tabledata[9]["30"]}}</td>
            <td style="text-align: center">{{tabledata[9]["31"]}}</td>
        </tr>
        <tr>
            <td style="text-align: center">11</td>
            <td colspan="4">{{tabledata[10]["content11"]}}</td>
            <td style="text-align: center">{{tabledata[10]["method11"]}}</td>
            <td style="text-align: center">{{tabledata[10]["1"]}}</td>
            <td style="text-align: center">{{tabledata[10]["2"]}}</td>
            <td style="text-align: center">{{tabledata[10]["3"]}}</td>
            <td style="text-align: center">{{tabledata[10]["4"]}}</td>
            <td style="text-align: center">{{tabledata[10]["5"]}}</td>
            <td style="text-align: center">{{tabledata[10]["6"]}}</td>
            <td style="text-align: center">{{tabledata[10]["7"]}}</td>
            <td style="text-align: center">{{tabledata[10]["8"]}}</td>
            <td style="text-align: center">{{tabledata[10]["9"]}}</td>
            <td style="text-align: center">{{tabledata[10]["10"]}}</td>
            <td style="text-align: center">{{tabledata[10]["11"]}}</td>
            <td style="text-align: center">{{tabledata[10]["12"]}}</td>
            <td style="text-align: center">{{tabledata[10]["13"]}}</td>
            <td style="text-align: center">{{tabledata[10]["14"]}}</td>
            <td style="text-align: center">{{tabledata[10]["15"]}}</td>
            <td style="text-align: center">{{tabledata[10]["16"]}}</td>
            <td style="text-align: center">{{tabledata[10]["17"]}}</td>
            <td style="text-align: center">{{tabledata[10]["18"]}}</td>
            <td style="text-align: center">{{tabledata[10]["19"]}}</td>
            <td style="text-align: center">{{tabledata[10]["20"]}}</td>
            <td style="text-align: center">{{tabledata[10]["21"]}}</td>
            <td style="text-align: center">{{tabledata[10]["22"]}}</td>
            <td style="text-align: center">{{tabledata[10]["23"]}}</td>
            <td style="text-align: center">{{tabledata[10]["24"]}}</td>
            <td style="text-align: center">{{tabledata[10]["25"]}}</td>
            <td style="text-align: center">{{tabledata[10]["26"]}}</td>
            <td style="text-align: center">{{tabledata[10]["27"]}}</td>
            <td style="text-align: center">{{tabledata[10]["28"]}}</td>
            <td style="text-align: center">{{tabledata[10]["29"]}}</td>
            <td style="text-align: center">{{tabledata[10]["30"]}}</td>
            <td style="text-align: center">{{tabledata[10]["31"]}}</td>
        </tr>
        <tr>
            <td colspan="6" style="text-align: center">开  动  台  时 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;全月<input type="text" style="width:30px;border: none;font-size:17px;text-align:center" v-model="state_time.running">小时</td>
            <td colspan="31" rowspan="2" style="text-align: center">注：每天将检点情况，按符号列入格内，良好"√"尚可"△"待修"×"已修"○"</td>
        </tr>
        <tr>
            <td colspan="2" rowspan="4" style="text-align: center">停开<br>台时</td>
            <td style="text-align: center">故障停台</td>
            <td colspan="3" style="text-align: right">全月<input type="text" style="width:30px;border: none;font-size:17px;text-align:center" v-model="state_time.fault">小时</td>
        </tr>
        <tr>
            <td style="text-align: center">其他原因</td>
            <td colspan="3" style="text-align: right">（待料、停电、调休、无任务）<br>全月<input type="text" style="width:30px;border: none;font-size:17px;text-align:center" v-model="state_time.other">小时</td>
            <td colspan="5" rowspan="5" style="text-align: center">设备状况与修理意见</td>
            <td colspan="26" rowspan="5"></td>
        </tr>
        <tr>
            <td style="text-align: center">保    养</td>
            <td colspan="3" style="text-align: right">全月<input type="text" style="width:30px;border: none;font-size:17px;text-align:center" v-model="state_time.mantain">小时</td>
        </tr> 
        <tr>
            <td style="text-align: center">计划小修</td>
            <td colspan="3" style="text-align: right">全月<input type="text" style="width:30px;border: none;font-size:17px;text-align:center" v-model="state_time.plan">小时</td>
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
import axios from "axios";
export default {
    name:"Checklist",
    data(){
        return {
            tabledata:
            [
                {"content1":"","method1":"","1":"","2":"","3":"","4":"","5":"","6":"","7":"","8":"","9":"","10":"","11":"","12":"","13":"","14":"","15":"","16":"","17":"","18":"","19":"","20":"","21":"","22":"","23":"","24":"","25":"","26":"","27":"","28":"","29":"","30":"","31":""},
                {"content2":"","method2":"","1":"","2":"","3":"","4":"","5":"","6":"","7":"","8":"","9":"","10":"","11":"","12":"","13":"","14":"","15":"","16":"","17":"","18":"","19":"","20":"","21":"","22":"","23":"","24":"","25":"","26":"","27":"","28":"","29":"","30":"","31":""},
                {"content3":"","method3":"","1":"","2":"","3":"","4":"","5":"","6":"","7":"","8":"","9":"","10":"","11":"","12":"","13":"","14":"","15":"","16":"","17":"","18":"","19":"","20":"","21":"","22":"","23":"","24":"","25":"","26":"","27":"","28":"","29":"","30":"","31":""},
                {"content4":"","method4":"","1":"","2":"","3":"","4":"","5":"","6":"","7":"","8":"","9":"","10":"","11":"","12":"","13":"","14":"","15":"","16":"","17":"","18":"","19":"","20":"","21":"","22":"","23":"","24":"","25":"","26":"","27":"","28":"","29":"","30":"","31":""},
                {"content5":"","method5":"","1":"","2":"","3":"","4":"","5":"","6":"","7":"","8":"","9":"","10":"","11":"","12":"","13":"","14":"","15":"","16":"","17":"","18":"","19":"","20":"","21":"","22":"","23":"","24":"","25":"","26":"","27":"","28":"","29":"","30":"","31":""},
                {"content6":"","method6":"","1":"","2":"","3":"","4":"","5":"","6":"","7":"","8":"","9":"","10":"","11":"","12":"","13":"","14":"","15":"","16":"","17":"","18":"","19":"","20":"","21":"","22":"","23":"","24":"","25":"","26":"","27":"","28":"","29":"","30":"","31":""},
                {"content7":"","method7":"","1":"","2":"","3":"","4":"","5":"","6":"","7":"","8":"","9":"","10":"","11":"","12":"","13":"","14":"","15":"","16":"","17":"","18":"","19":"","20":"","21":"","22":"","23":"","24":"","25":"","26":"","27":"","28":"","29":"","30":"","31":""},
                {"content8":"","method8":"","1":"","2":"","3":"","4":"","5":"","6":"","7":"","8":"","9":"","10":"","11":"","12":"","13":"","14":"","15":"","16":"","17":"","18":"","19":"","20":"","21":"","22":"","23":"","24":"","25":"","26":"","27":"","28":"","29":"","30":"","31":""},
                {"content9":"","method9":"","1":"","2":"","3":"","4":"","5":"","6":"","7":"","8":"","9":"","10":"","11":"","12":"","13":"","14":"","15":"","16":"","17":"","18":"","19":"","20":"","21":"","22":"","23":"","24":"","25":"","26":"","27":"","28":"","29":"","30":"","31":""},
                {"content10":"","method10":"","1":"","2":"","3":"","4":"","5":"","6":"","7":"","8":"","9":"","10":"","11":"","12":"","13":"","14":"","15":"","16":"","17":"","18":"","19":"","20":"","21":"","22":"","23":"","24":"","25":"","26":"","27":"","28":"","29":"","30":"","31":""},
                {"content11":"","method11":"","1":"","2":"","3":"","4":"","5":"","6":"","7":"","8":"","9":"","10":"","11":"","12":"","13":"","14":"","15":"","16":"","17":"","18":"","19":"","20":"","21":"","22":"","23":"","24":"","25":"","26":"","27":"","28":"","29":"","30":"","31":""}
            ],
            state_time:
            {
                    "running":"","fault":"","other":"","mantain":"","plan":""
            },
            header:{
                "workshop":"",
                "group":"",
                "year_month":"",
                "e_number":"",
                "e_name":"",
                "e_type":"",
                "e_user":""
            }
        }
    },
    created() {
        
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