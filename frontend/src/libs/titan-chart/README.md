# Titan Chart

### Features

`@fe/titan-chart` 是一款运行在 `vue 2.x` 环境下的 `Apache ECharts 5.x` 组件，基于 [vue-echarts](https://github.com/ecomfe/vue-echarts) 进行的二次开发

- `@fe/titan-chart` 通过 `use` 安装插件的方式来实现 `echarts` 皮肤、地图、默认参数的配置

- `@fe/titan-chart` 无缝对接 `echarts` 文档板块；

### Quick Start

- 全局引用（推荐）

```javascript
// >>> main.js
import Vue from 'vue'

import TitanChart from '@fe/titan-chart'
import themeJSON from '@/theme/echarts-theme.js'
import { LineOption, BarOption } from '@/assets/chart-options'

Vue.use(TitanChart, {
  notMerge: true,
  autoResize: true,
  defaultTheme: 'red',
  themes: [
    { name: 'red', theme: themeJSON.theme }
  ],
  presets: [
    { name: 'line', option: LineOption },
    { name: 'bar', option: BarOption }
  ]
});
```

- 组件内引入

```js
import TitanChart from '@fe/titan-chart'
export default {
    components: {
        TitanChart
    }
}
```

### Global Config 全局配置项

`TitanChart` 支持在 `main.js` 中使用 `Vue.use(TitanChart[, option])` 的方式配置参数，具体参数如下：

|     参数     | 数据类型 | 默认值 |                                            示例                                            |        说明        |
| :----------: | :------: | :----: | :----------------------------------------------------------------------------------------: | :----------------: |
| defaultTheme |  String  |   -    |                                          'custom'                                          |    默认主题名称    |
|    themes    |  Array   |   []   | [{ <br> name: 名称*string*, <br> theme: 内容*object*, <br> extra: 其他配置*object* <br> }] | 需要注册的主题列表 |
|     maps     |  Array   |   []   | [{ <br> name: 名称*string*, <br> map: GeoJson 格式的数据, <br> specialAreas:可选。将地图中的部分区域缩放到合适的位置  <br> }] | 需要注册的地图列表，参考 [echarts.registerMap](https://echarts.apache.org/zh/api.html#echarts.registerMap) |
|   presets    |  Array   |   []   |  [{ <br> name: 名称*string*, <br> option: echarts option*objedt* <br>}]                                                                                          | 需要预置的配置列表 |

### Attributes/Props 属性

使用方式： `<titan-chart preset="red" :option="calcOption" ... />`

|     参数     | 数据类型 |         默认值         |   可选值   |                                              说明                                              |
| :----------: | :------: | :--------------------: | :--------: | :--------------------------------------------------------------------------------------------: |
|    preset    |  String  |       undefined        |     -      |                        要使用的默认配置名称,默认配置使用 `presets` 导入                        |
|    option    |  Object  |           {}           |     -      | chart的配置，可采用官方提供的结构也可使用以“_”开头的结构将与默认参数特殊合并，需配合preset使用 |
|    width     |  String  |          100%          |     -      |                                            图表宽度                                            |
|    height    |  String  |          100%          |     -      |                                            图表高度                                            |
|    theme     |  String  |       undefined        |     -      |                         已经通过 echarts.registerTheme 注册的主题名称                          |
|    group     |  String  |       undefined        |     -      |                                      图表的分组，用于联动                                      |
| initOptions  |  Object  | { renderer: 'canvas' } |     -      |                  附加参数，用于配置devicePixelRatio，renderer，width，height                   |
|    group     |  String  |       undefined        |     -      |                                      图表的分组，用于联动                                      |
| watchShallow | Boolean  |         false          | false/true |                                      watch不使用deep属性                                       |
| manualUpdate | Boolean  |         false          | false/true |                                        手动控制图表更新                                        |
|   notMerge   | Boolean  |         false          | false/true |                               是否不跟之前设置的 option 进行合并                               |
|  autoResize  | Boolean  |         false          | false/true |                                      当尺寸变化时自动重绘                                      |

### Util Methods 工具方法

#### getThemeOption

**说明**

`getThemeOption(theme, path)`
获取主题配置JSON，该JSON在`titan-chart`注册时设置`themes`中的`theme`

**参数**

- **theme(string)**
主题名称，必填

- **path(string)**
属性路径，设置该值则返回主题JSON该路径下的值，否则返回全部

**返回值**

- **(*)**: 任意值

#### getExtraOption

**说明**

`getExtraOption(theme, path)`
获取主题额外配置的JSON，该JSON在`titan-chart`注册时设置`themes`中的`extra`

**参数**

- **theme(string)**
主题名称，必填

- **path(string)**
属性路径，设置该值则返回主题JSON该路径下的值，否则返回全部

**返回值**

- **(*)**: 任意值

### Usage Example 使用示例

- 预先配置好的 ECharts Option

```javascript
export default {
  tooltip: {
    trigger: 'axis',
    backgroundColor: '#fff',
    position: 'top',
    textStyle: {
      fontSize: 12,
      color: '#606266',
      align: 'left'
    }
  },
  grid: {
    left: '3%',
    right: '6%',
    top: '3%',
    bottom: '3%',
    containLabel: true
  },
  xAxis: {
    type: 'category',
    boundaryGap: false,
    axisLine: {
      lineStyle: {
        color: '#efefef',
        width: 1
      }
    },
    axisLabel: {
      color: '#909399',
      showMaxLabel: true,
      textStyle: {
        fontSize: 12,
        fontFamily: 'Microsoft YaHei'
      }
    },
    axisTick: { show: false },
  },
  yAxis: {
    type: 'value',
    axisLine: { show: false },
    axisLabel: {
      color: '#909399',
      textStyle: { fontSize: 12 },
      formatter: value => {
        if (value >= 1000) {
          return (value / 1000) + 'K'
        } else {
          return value
        }
      }
    },
    splitLine: { lineStyle: { color: '#ebeef5' } },
    axisTick: { show: false }
  },
  series: [{
    smooth: true,
    symbol: 'circle',
    type: 'line',
    lineStyle: { color: '#5a7acc' }
  }]
}
```

- `main.js` 全局引入

```js
import Vue from 'vue';
import TitanChart from '@fe/titan-chart';

// 引入预设Option
import LineOption from './chart-options/lineChart';

// 引入 ECharts Theme JSON 文件 （由官网主题生成工具制作）
import redThemeJSON from '@/themes/echarts-archaism.json';
import blueThemeJSON from '@/themes/echarts-business.json';

// 官方主题工具提供的配置如果不够，可以再额外设置 Option JSON
import { redExtra, blueExtra } from '@/themes/echarts-extra-option';

Vue.use(TitanChart, {
  notMerge: true,
  autoResize: true,
  theme: 'red'
  themes: [
    { 
      name: redThemeJSON.themeName,
      theme: redThemeJSON.theme,
      extra: redExtra 
    },
    { 
      name: blueThemeJSON.themeName,
      theme: blueThemeJSON.theme,
      extra: blueExtra
    }
  ],
  presets: [
    { name: 'custom-line', option: LineOption }
  ]
});
```

- 实际使用

```jsx
/**
 * template使用示例
 */
<titan-chart
  :option="option"
  preset="custom-line"
  theme="blue"
/>
```
