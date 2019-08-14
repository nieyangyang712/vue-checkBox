<template>
    <div>
        <!-- 列表信息 -->    
        <div class="listfile">
           <Table border ref="selection" :columns="stationInfoColumns" :height="500" :data="page_Data3" @on-select-all='selectTable' @on-select='selectTable' @on-select-cancel='selectTable'></Table>
        </div>
    </div>
</template>

<script>
import {mpidCode} from 'src/utils/serialPort.js';
  export default {
   data () {
        return {
            stationInfoColumns:[
               {type: 'selection', width: 50,align: 'left'},
               {title: '线路',key: 'line', width: 80,align: 'center',
                    render: (h,params)=> {
                        return h('div',params.row.line.lineName)
                    }
               },
               {title: '车站',key: 'stationInfo',
                    render:(h,params)=> {
                        return h('div',[
                            h('Checkbox',{
                                props: {
                                    value: params.row.checkedAll,
                                    
                                    },
                                on:{
                                    "on-change": (event) => {
                                        if (event) {
                                            console.log("event",event);
                                            for(let i= 0; i<params.row.stationInfo.length; i++ ){
                                                params.row.checked.push(params.row.stationInfo[i].tccStationId);
                                            }
                                            console.log("1",params.row.checked);
                                            // console.log("2",params.row.stationInfo)
                                        } else {
                                            params.row.checked = [];
                                        }
                                    }
                                }
                            }, '全选'), 
                            h('CheckboxGroup',
                            {   props: {value: params.row.checked},
                                on:{
                                    "on-change":(val)=>{
                                        params.row.checked = val;
                                        params.row.checkedAll = params.row.checked.length === params.row.stationInfo.length ? true:false;
                                        console.log("checks",val);
                                    }
                                }
                            
                            },
                            (params.row.stationInfo || []).map(v=> {
                                return h('Checkbox', {
                                    props: { 
                                        label: v.tccStationId,
                                        }
                                    }, v.stationSname)
                            } )
                        )])
                    }
               },
               {title: '操做',key: 'action',width: 200,align: 'center',fixed:'right',
                        render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: {
                                        type: 'primary', size: 'small'
                                    },
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                            // console.log(params.row)
                                      }
                                    }
                                }, '详情'),
                                h('Button', {
                                    props: {
                                        type: 'primary', size: 'small'
                                    },
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                        }
                                    }
                                }, '编辑'),
                                h('Button', {
                                    props: {
                                        type: 'error', size: 'small'
                                    },
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                           }
                                        }
                                }, '删除'),
                            ])
                        }
               },

            ],
                //列表初始数据
                page_Data3: [
                    {
                        "line":{ "lineId":"01","lineName":"1号线"},
                        "stationInfo":[{ "tccStationId":"0101","stationSname":"预留"},{"tccStationId":"0102","stationSname":"预留"},{"tccStationId":"0103","stationSname":"苹果园",}],
                        "checked": [{ "tccStationId":"0101","stationSname":"预留"}], 
                        "checkedAll":false },
                    {
                        "line":{ "lineId":"02","lineName":"2号线"},
                        "stationInfo":[{ "tccStationId":"0201","stationSname":"西直门"},{"tccStationId":"0202","stationSname":"车公庄"},{"tccStationId":"0203","stationSname":"阜成门"}],
                        "checked": [], 
                        "checkedAll":false }
                ],
              }
      },
    methods: {
        //列表左侧checkbox
        selectTable(data){ 
            this.selectData = data;
        },
    },
    created(){
    },
    mounted(){
    },

  }
</script>

<style>

</style>
