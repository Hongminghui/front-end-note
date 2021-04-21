## 1. 入门例子

一个节点渲染多张图（如下)，一个页面同时有多张图用多个节点即可

```html
<!DOCTYPE html>
<html lang="zh">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>多系列混合图表</title>
    <script src="https://cdn.jsdelivr.net/npm/echarts@4.7.0/dist/echarts.js"></script>
    <style>
        #chart {
            height: 400px;
            width: 800px;
        }
    </style>
</head>

<body>
    <div id="chart"></div>
    <script>
        const chartDom = document.getElementById('chart')
        const chart = echarts.init(chartDom)
        chart.setOption({
            title: {
                text: "Echarts 多系列案例"
            },
            xAxis: {
                data: ['一季度', '二季度', '三季度', '四季度']
            },
            yAxis: {},
            series: [
                {
                    type: 'pie',    //饼状图
                    center: ['65%', 60],//饼图中心点位置
                    radius: 35,
                    data: [
                        {
                            name: '分类一', value: 50
                        },
                        {
                            name: '分类二', value: 60
                        },
                        {
                            name: '分类三', value: 55
                        },
                        {
                            name: '分类四', value: 70
                        },
                    ]
                },
                {
                    type: 'line',   //折线图
                    data: ['100', '112', '96', '123']
                },
                {
                    type: 'bar',    //柱状图
                    data: ['79', '81', '88', '72']
                }
            ]
        })
    </script>
</body>

</html>
```

