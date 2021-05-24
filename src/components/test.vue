<template>
    <div id="echartpage">
        <div class="title">智能推荐预测</div>
        <div id="circle" style='width:800px;height:300px;background:#fff'></div>
        <div id="bar" style='width:800px;height:500px;background:#fff'></div>
        <div id="circle1" style='width:800px;height:300px;background:#fff'></div>
        <div id="bar1" style='width:800px;height:500px;background:#fff'></div>
    </div>

</template>

<script>
import * as echarts from 'echarts';
import dataEcharts from './data.js';
export default {
    name: 'HelloWorld',
    data() {
        return {
            myChart: null,
            myChart1: null,
            myChart2: null,
            myChart3: null,
            totalPsScore: { "PS81-90": 0.0, "PS91-100": 0.0, "PS71-80": 0.19047619, "PSCount": 21.0, "PS60-70": 0.14285715 },
            totalCcScore: { "CC0.7-0.8": 0.0, "CC0.6-0.7": 0.04761905, "CC0.8-0.9": 0.0, "CCCount": 21.0, "CC0.9-1": 0.0 },
            psList: [
                { "algorithmType": "101005", "algorithmName": "最优子集", "dataType": "020005", "dataName": "DERF2.0逐日", "observeMonthday": "0411", "observeYear": 2021, "psScore": 80.0, "ccScore": 0.56 },
                { "algorithmType": "101005", "algorithmName": "最优子集", "dataType": "020005", "dataName": "DERF2.0逐日", "observeMonthday": "0410", "observeYear": 2021, "psScore": 77.14, "ccScore": 0.61 },
                { "algorithmType": "101005", "algorithmName": "最优子集", "dataType": "020005", "dataName": "DERF2.0逐日", "observeMonthday": "0416", "observeYear": 2021, "psScore": 75.16, "ccScore": 0.3 },
                { "algorithmType": "101002", "algorithmName": "非参数百分位", "dataType": "020005", "dataName": "DERF2.0逐日", "observeMonthday": "0416", "observeYear": 2021, "psScore": 74.36, "ccScore": -0.21 },
                { "algorithmType": "101013", "algorithmName": "主成分分析", "dataType": "020005", "dataName": "DERF2.0逐日", "observeMonthday": "0416", "observeYear": 2021, "psScore": 62.8, "ccScore": 0.23 },
                { "algorithmType": "101002", "algorithmName": "非参数百分位", "dataType": "020005", "dataName": "DERF2.0逐日", "observeMonthday": "0411", "observeYear": 2021, "psScore": 62.31, "ccScore": -0.25 },
                { "algorithmType": "101002", "algorithmName": "非参数百分位", "dataType": "020005", "dataName": "DERF2.0逐日", "observeMonthday": "0410", "observeYear": 2021, "psScore": 61.54, "ccScore": 0.07 },
                { "algorithmType": "101013", "algorithmName": "主成分分析", "dataType": "020005", "dataName": "DERF2.0逐日", "observeMonthday": "0410", "observeYear": 2021, "psScore": 59.9, "ccScore": 0.22 },
                { "algorithmType": "101013", "algorithmName": "主成分分析", "dataType": "020005", "dataName": "DERF2.0逐日", "observeMonthday": "0411", "observeYear": 2021, "psScore": 53.33, "ccScore": 0.06 },
                { "algorithmType": "101011", "algorithmName": "朴素贝叶斯", "dataType": "020005", "dataName": "DERF2.0逐日", "observeMonthday": "0411", "observeYear": 2021, "psScore": 50.0, "ccScore": 0.06 }
            ],
            ccList: [
                { "algorithmType": "101005", "algorithmName": "最优子集", "dataType": "020005", "dataName": "DERF2.0逐日", "observeMonthday": "0410", "observeYear": 2021, "psScore": 77.14, "ccScore": 0.61 },
                { "algorithmType": "101013", "algorithmName": "主成分分析", "dataType": "020005", "dataName": "DERF2.0逐日", "observeMonthday": "0416", "observeYear": 2021, "psScore": 62.8, "ccScore": 0.23 },
                { "algorithmType": "101002", "algorithmName": "非参数百分位", "dataType": "020005", "dataName": "DERF2.0逐日", "observeMonthday": "0410", "observeYear": 2021, "psScore": 61.54, "ccScore": 0.07 },
                { "algorithmType": "101011", "algorithmName": "朴素贝叶斯", "dataType": "020005", "dataName": "DERF2.0逐日", "observeMonthday": "0411", "observeYear": 2021, "psScore": 50.0, "ccScore": 0.06 }
            ],
            newPsScore: [],
            newCcScore: [],
            newpsList: [],
            newccList: []
        }
    },
    mounted() {
        this.getData()
        var option1 = {
            dataset: {
                source: []
            },
            title: {
                text: '各类资料70以上分布',
                textStyle: {
                    color: '#dfdfe6'
                },
            },
            toolbox: {
                feature: {
                    saveAsImage: {
                    }
                }
            },
            color: ['#ce6dfa', '#eff08c', '#69dcab', '#2a84ed'],
            backgroundColor: '#262b58',
            series: [
                {
                    type: 'pie',
                    radius: [0, 100],
                    center: ['50%', '50%'],
                    label: {
                        formatter: '{b}\n({d}%)',
                    }
                }
            ]
        }
        option1.dataset.source = this.newPsScore
        this.myChart1 = echarts.init(document.getElementById('circle'));
        this.myChart1.setOption(option1);
        var option = {
            dataset: {
                source: []
            },
            backgroundColor: '#262b58',
            textStyle: {
                color: '#92a8c7'
            },
            color: {
                type: 'linear',
                x: 0,
                y: 0,
                x2: 1,
                y2: 0,
                colorStops: [{
                    offset: 0, color: '#2680f7'
                }, {
                    offset: 1, color: '#ad5eee'
                }],
                globalCoord: false
            },
            toolbox: {
                feature: {
                    saveAsImage: {
                    }
                }
            },
            grid: {
                x: 120,
                y: 40,
                x2: 60,
                y2: 50,
                borderWidth: 10
            },
            title: {
                text: '各类算法70以上分布',
                textStyle: {
                    color: '#dfdfe6'
                },
            },
            tooltip: {},
            xAxis: {
            },
            yAxis: { type: 'category' },
            series: {
                type: 'bar',
                encode: {x: 6, y: 1}
            }
        };
        option.dataset.source = this.newpsList
        this.myChart = echarts.init(document.getElementById('bar'));
        this.myChart.setOption(option);
        // cc饼图
        var option2 = {
            dataset: {
                source: []
            },
            title: {
                text: '各类资料70以上分布',
                textStyle: {
                    color: '#dfdfe6'
                },
            },
            toolbox: {
                feature: {
                    saveAsImage: {
                    }
                }
            },
            color: ['#ce6dfa', '#eff08c', '#69dcab', '#2a84ed'],
            backgroundColor: '#262b58',
            series: [
                {
                    type: 'pie',
                    radius: [0, 100],
                    center: ['50%', '50%'],
                    label: {
                        formatter: '{b}\n({d}%)',
                    }
                }
            ]
        }
        option2.dataset.source = this.newCcScore
        this.myChart2 = echarts.init(document.getElementById('circle1'));
        this.myChart2.setOption(option2);
        var option3 = {
            dataset: {
                source: []
            },
            backgroundColor: '#262b58',
            textStyle: {
                color: '#92a8c7'
            },
            color: {
                type: 'linear',
                x: 0,
                y: 0,
                x2: 1,
                y2: 0,
                colorStops: [{
                    offset: 0, color: '#2680f7'
                }, {
                    offset: 1, color: '#ad5eee'
                }],
                globalCoord: false
            },
            toolbox: {
                feature: {
                    saveAsImage: {
                    }
                }
            },
            grid: {
                x: 120,
                y: 40,
                x2: 60,
                y2: 50,
                borderWidth: 10
            },
            title: {
                text: '各类算法70以上分布',
                textStyle: {
                    color: '#dfdfe6'
                },
            },
            tooltip: {},
            xAxis: {
            },
            yAxis: { type: 'category' },
            series: {
                type: 'bar',
                encode: {x: 7, y: 1}
            }
        };
        option3.dataset.source = this.newccList
        this.myChart3 = echarts.init(document.getElementById('bar1'));
        this.myChart3.setOption(option3);
    },
    methods: {
        getData() {
            for (var x in this.totalPsScore) {
                this.newPsScore.push({ 'name': x, 'count': this.totalPsScore[x] })
            }
            this.newPsScore = this.newPsScore.filter(item=>{
                return item.name!='PSCount'
            })
            this.newpsList = this.psList.sort(this.sortby)
            for (var x in this.totalCcScore) {
                this.newCcScore.push({ 'name': x, 'count': this.totalCcScore[x] })
            }
            this.newCcScore = this.newCcScore.filter(item=>{
                return item.name!='CCCount'
            })
            
            this.newccList = this.ccList.filter(item=>{
                return item.algorithmName!=null
            })
            this.newccList = this.newccList.sort(this.sortby1)
            console.log(this.newccList)
        },
        sortby(a,b) {
            return a.psScore-b.psScore
        },
        sortby1(a,b) {
            return a.ccScore-b.ccScore
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#echartpage {
    background: #262b58;
}
.title {
    color: #0ab1d6;
    font-size: 24px;
}
h1,
h2 {
    font-weight: normal;
}
ul {
    list-style-type: none;
    padding: 0;
}
li {
    display: inline-block;
    margin: 0 10px;
}
a {
    color: #42b983;
}
</style>
