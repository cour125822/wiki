---
title: echarts
description: 
published: true
date: 2023-03-26T08:10:33.500Z
tags: 
editor: markdown
dateCreated: 2023-02-26T04:15:30.254Z
---

### **安装**

```
npm install echarts vue-echarts
```

### **配置**

```
// main.js
import 'echarts'
import ECharts from 'vue-echarts'
app.component('v-chart', ECharts)
```

### **使用**

```
<template>
    <div class="home">
        <v-chart :option="option_column" style="height: 400px"></v-chart>
    </div>
</template>

<script>
// import导入组件

export default {
    name: 'IndeX',
    component: {
    },
    setup () {
        let option_column = {
            title: { text: "宇宙女主" },
            tooltip: {},
            xAxis: {
                data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"],
            },
            yAxis: {},
            series: [{
                name: "销量",
                type: "bar",
                data: [5, 20, 36, 10, 10, 20],
            },],
        }
        return {
            option_column
        }
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.show {
    width: 500px;
    height: 500px;
    border: 1px solid red;
}
.echarts {
    width: 100%;
    height: 100%;
}
</style>

```