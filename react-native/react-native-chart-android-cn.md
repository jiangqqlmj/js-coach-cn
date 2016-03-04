####项目名称:React Native Chart For Android 图表开源项目
####项目来源:https://github.com/hongyin163/react-native-chart-android
# react-native-chart-android
react-native-chart-android提供为android模块添加图表,图表都来自mpandroidchart开源组件库,包括柱状图、折线图、组合图等。
关于MpAndroidChart更多详细,你可以阅读下面的文档:

-[**MPAndroidChart-github**](https://github.com/PhilJay/MPAndroidChart/) 
-[**MPAndroidChart-Wiki**](https://github.com/PhilJay/MPAndroidChart/wiki) 


### 配置安装

```
npm install react-native-chart-android --save
```

### 添加组件到Android项目中

* 在Android项目`android/setting.gradle`作如下修改

```gradle
include ':react-native-chart-android'
project(':react-native-chart-android').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-chart-android/android')
```

* 修改build.gradlde文件，具体路径`android/app/build.gradle`

```gradle
...
dependencies {
  ...
  compile project(':react-native-chart-android')
}
```

* 在MainActiivty.java中注册模块

```java
import cn.mandata.react_native_mpchart.MPChartPackage;  // <--- import

public class MainActivity extends Activity implements DefaultHardwareBackBtnHandler {
  ......

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    mReactRootView = new ReactRootView(this);

    mReactInstanceManager = ReactInstanceManager.builder()
      .setApplication(getApplication())
      .setBundleAssetName("index.android.bundle")
      .setJSMainModuleName("index.android")
      .addPackage(new MainReactPackage())
      .addPackage(new MPChartPackage()) // <------ add this line to yout MainActivity class
      .setUseDeveloperSupport(BuildConfig.DEBUG)
      .setInitialLifecycleState(LifecycleState.RESUMED)
      .build();

    mReactRootView.startReactApplication(mReactInstanceManager, "AndroidRNSample", null);

    setContentView(mReactRootView);
  }

  ......

}
```

## 实例代码
```javascript
/* @flow */
'use strict';

var React = require('react-native');
var TitleBar=require('./TitleBar');
var {
  BarChart,
  CombinedChart
}=require('../index.android');
var {
  StyleSheet,
  View,
  Text
} = React;

var Component = React.createClass({
  getBarData:function (argument) {
    var data={
      xValues:['1','2','3'],
      yValues:[
        {
          data:[4.0,5.0,6.0],
          label:'test1',
          config:{
            color:'blue'
          }
        },
        {
          data:[4.0,5.0,6.0],
          label:'test2',
          config:{
            color:'red'
          }
        },
        {
          data:[4.0,5.0,6.0],
          label:'test2',
          config:{
            color:'yellow'
          }
        }
      ]
    };  
    return data;
  },
  getRandomData:function (argument) {
    var data={};
    data['xValues']=[];
    data['yValues']=[
      {
        data:[],
        label:'test1',
        config:{
          color:'blue'
        }
      }
    ];
    for (var i = 0; i < 500; i++) {
      data.xValues.push(i+'');
      data.yValues[0].data.push(Math.random()*100);
    };
    return data;
  },
  render: function() {
    return (
      <View style={styles.container}>
        <TitleBar/>
        <View style={styles.chartContainer}>
          <BarChart style={{flex:1}} data={this.getBarData()}/>
          <BarChart 
            style={{flex:1}} 
            data={this.getRandomData()}
            visibleXRange={[0,30]}
            maxVisibleValueCount={50} 
                xAxis={{drawGridLines:false,gridLineWidth:1,position:"BOTTOM"}}
                yAxisRight={{enable:false}} 
                yAxis={{startAtZero:false,drawGridLines:false,position:"INSIDE_CHART"}}
                drawGridBackground={false}
                backgroundColor={"WHITE"} 
                description={"测试"}
                legend={{enable:true,position:'ABOVE_CHART_LEFT',direction:"LEFT_TO_RIGHT"}}
            />
        </View>
      </View>
    );
  }
});

var styles = StyleSheet.create({
  container:{
    flex:1
  },
  chartContainer:{
    flex:1
  },
  chart:{
    flex:1
  }
});


module.exports = Component;

```
![alt tag](https://github.com/hongyin163/react-native-chart-android/blob/master/sample/chart1.JPG)

![alt tag](https://github.com/hongyin163/react-native-chart-android/blob/master/sample/chart2.JPG)

在Sample实例文件夹中还有很多例子可以运行演示，具体可以下载运行一下
## 协议
MIT
