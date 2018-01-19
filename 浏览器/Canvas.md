Canvas是HTML5新增的组件，它就像一块幕布，可以用JavaScript在上面绘制各种图表、动画等.

一个Canvas定义了一个指定尺寸的矩形框，在这个范围内我们可以随意绘制：
```
<canvas id="test-canvas" width="300" height="200"></canvas>
```

由于浏览器对HTML5标准支持不一致，所以，通常在<canvas>内部添加一些说明性HTML代码，如果浏览器支持Canvas，它将忽略<canvas>内部的HTML，如果浏览器不支持Canvas，它将显示<canvas>内部的HTML：
```
<canvas id="test-stock" width="300" height="200">
    <p>Current Price: 25.51</p>
</canvas>
```

getContext('2d')方法让我们拿到一个CanvasRenderingContext2D对象，所有的绘图操作都需要通过这个对象完成。
```
var ctx = canvas.getContext('2d');
```

如果需要绘制3D怎么办？HTML5还有一个WebGL规范，允许在Canvas中绘制3D图形：

```
gl = canvas.getContext("webgl");
```

### 绘制形状

我们可以在Canvas上绘制各种形状。在绘制前，我们需要先了解一下Canvas的坐标系统：

![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001436926614788af8f274570d54736bddbbf7b2b03a9eb000/l)

Canvas的坐标以左上角为原点，水平向右为X轴，垂直向下为Y轴，以像素为单位，所以每个点都是非负整数。

CanvasRenderingContext2D对象有若干方法来绘制图形：

```
/* JavaScript */

'use strict';

var
    canvas = document.getElementById('test-shape-canvas'),
    ctx = canvas.getContext('2d');
    
ctx.clearRect(0, 0, 200, 200); // 擦除(0,0)位置大小为200x200的矩形，擦除的意思是把该区域变为透明
ctx.fillStyle = '#dddddd'; // 设置颜色
ctx.fillRect(10, 10, 130, 130); // 把(10,10)位置大小为130x130的矩形涂色
// 利用Path绘制复杂路径:
var path=new Path2D();
path.arc(75, 75, 50, 0, Math.PI*2, true);
path.moveTo(110,75);
path.arc(75, 75, 35, 0, Math.PI, false);
path.moveTo(65, 65);
path.arc(60, 65, 5, 0, Math.PI*2, true);
path.moveTo(95, 65);
path.arc(90, 65, 5, 0, Math.PI*2, true);
ctx.strokeStyle = '#0000ff';
ctx.stroke(path);

```

```
<!-- HTML -->
<canvas id="test-shape-canvas" width="200" height="200" style="border: 1px solid #ccc; margin-top: 15px;">
</canvas>
```

### 绘制文本

绘制文本就是在指定的位置输出文本，可以设置文本的字体、样式、阴影等，与CSS完全一致：
```
/* JavaScript */

'use strict';

var
    canvas = document.getElementById('test-text-canvas'),
    ctx = canvas.getContext('2d');
    
ctx.clearRect(0, 0, canvas.width, canvas.height);
ctx.shadowOffsetX = 2;
ctx.shadowOffsetY = 2;
ctx.shadowBlur = 2;
ctx.shadowColor = '#666666';
ctx.font = '24px Arial';
ctx.fillStyle = '#333333';
ctx.fillText('带阴影的文字', 20, 40);

```
```
<!-- HTML -->

<canvas id="test-text-canvas" width="300" height="100" style="border: 1px solid #ccc; margin-top: 15px;">
</canvas>
```


Canvas除了能绘制基本的形状和文本，还可以实现动画、缩放、各种滤镜和像素转换等高级操作。如果要实现非常复杂的操作，考虑以下优化方案：

- 通过创建一个不可见的Canvas来绘图，然后将最终绘制结果复制到页面的可见Canvas中；

- 尽量使用整数坐标而不是浮点数；

- 可以创建多个重叠的Canvas绘制不同的层，而不是在一个Canvas中绘制非常复杂的图；

- 背景图片如果不变可以直接用<img>标签并放到最底层。


### 练习

请根据从163获取的JSON数据绘制最近30个交易日的K线图，数据已处理为包含一组对象的数组：

```
'use strict';

window.loadStockData = function (r) {
    var
        NUMS = 30,
        data = r.data;
    if (data.length > NUMS) {
        data = data.slice(data.length - NUMS);
    }
    data = data.map(function (x) {
        return {
            date: x[0],
            open: x[1],
            close: x[2],
            high: x[3],
            low: x[4],
            vol: x[5],
            change: x[6]
        };
    });
    window.drawStock(data);
}

window.drawStock = function (data) {

    var
        canvas = document.getElementById('stock-canvas'),
        width = canvas.width,
        height = canvas.height,
        ctx = canvas.getContext('2d');
    console.log(JSON.stringify(data[0])); // {"date":"20150602","open":4844.7,"close":4910.53,"high":4911.57,"low":4797.55,"vol":62374809900,"change":1.69}
    ctx.clearRect(0, 0, width, height);
    ctx.fillText('Test Canvas', 10, 10);
    
};

// 加载最近30个交易日的K线图数据:
var js = document.createElement('script');
js.src = 'http://img1.money.126.net/data/hs/kline/day/history/2015/0000001.json?callback=loadStockData&t=' + Date.now();
document.getElementsByTagName('head')[0].appendChild(js);
```

别人家的孩子的答案：
```
var
    canvas = document.getElementById('stock-canvas'),
    width = canvas.width,
    height = canvas.height,
    ctx = canvas.getContext('2d');
console.log(JSON.stringify(data[0])); // {"date":"20150602","open":4844.7,"close":4910.53,"high":4911.57,"low":4797.55,"vol":62374809900,"change":1.69}
ctx.clearRect(0, 0, width, height);
ctx.fillText('Test Canvas', 10, 10);

    var max=data.reduce((x,y)=>(x.high>y.high?x:y)).high;
    var min=data.reduce((x,y)=>(x.low<y.low?x:y)).low;
    var step=width/data.length;
    //alert(max+','+min+','+step);
    data.forEach(function(element,index){
    //alert(element.high+','+element.low);
    var majHeight=0,start=0,minHeight=element.high - element.low;
    if(element.open < element.close){
         ctx.fillStyle='red';
         start=element.close;
         majHeight=element.close - element.open;             
    }
    else{
         ctx.fillStyle='green';
         start=element.open;
         majHeight=element.open - element.close;
    }
    var x1=index*step,y1=(max-start)/(max-min)*height;
    var x2=x1+step/2,y2=(max-element.high)/(max-min)*height;
    var h1=majHeight/(max-min)*height,h2=minHeight/(max-min)*height;
    ctx.fillRect(x1,y1,step,h1);//rect
    ctx.beginPath();//line
    ctx.strokeStyle=ctx.fillStyle;
    ctx.moveTo(x2,y2);
    ctx.lineTo(x2,y2+h2);
    ctx.stroke(); 
});
/*
    var
            high = 0,
            low = 100000,

            left,right,top, bottom,  //绘图的边界    

            tsize = 20;

            ctx.clearRect(0, 0, width, height);

            //计算最大最小
            data.forEach(function(item){
                if (eval(item.high)> eval(high)) {
                    high = item.high;
                }

                if (eval(item.low) < eval(low)) {
                    low = item.low;
                }
            })

            //设置绘制的边界
            left = 0;
            right = width - tsize * 2;
            top = 0;
            bottom = height - tsize;

            //绘制坐标轴
            ctx.strokeStyle = '#000000';
            ctx.beginPath();
            ctx.moveTo(left, bottom);
            ctx.lineTo(right, bottom);
            ctx.lineTo(right, top);
        //    ctx.closePath();
            ctx.stroke();

            //绘制X坐标轴标签
            data.forEach(function(item, i, arr){
                if (i % 5 === 0) {
                    ctx.fillText(item.date.substring(4), left+ (right - left) * i / data.length, bottom+ tsize);
                }
            })
            //绘制Y坐标轴标签
            for (var i = 0; i < 5; i++) {
                ctx.fillText(low + (high - low) / 5 * i, right, bottom - (bottom - top) / 5 * i);
            }

            // 蜡烛间隙0.5
            var candleWidth = (right - left) / (data.length * 1.5); 
            var heightPerPrice = (bottom - top) / (high - low);

            //绘制蜡烛图
            data.forEach(function(item, i, arr){
                //定位坐标
                if (eval(item.close)> eval(item.open)) {
                    ctx.strokeStyle = '#ff0000';
                }else{
                    ctx.strokeStyle = '#00ff00';
                }

                console.log('x:'+ getX(i) + ' y:' + getY(Math.max(item.open, item.close)) + ' width:' + candleWidth + ' height:' + (Math.abs(item.open- item.close) * heightPerPrice));

                //绘制矩形
                ctx.strokeRect(getX(i), getY(Math.max(item.open, item.close)), candleWidth, Math.abs(item.open - item.close) * heightPerPrice);
                //绘制高低
                ctx.beginPath();
                ctx.moveTo(getX(i) + candleWidth / 2, getY(item.high));
                ctx.lineTo(getX(i) + candleWidth / 2, getY(Math.max(item.open, item.close)));
                ctx.moveTo(getX(i) + candleWidth / 2, getY(Math.min(item.open, item.close)));
                ctx.lineTo(getX(i) + candleWidth / 2, getY(item.low));
                ctx.stroke();
            });



            function getX(index){
                return candleWidth * index * 1.5;
            }

            function getY(price){
                return bottom - Math.abs((price - low) * heightPerPrice);
            }

*/
/*
 // 画上背景
    ctx.fillStyle = '#000000';
    ctx.fillRect(0, 0, width, height);

    // 得出数据中的最大和最小值
    //var highest = data[0].high;
    //var lowest = data[0].low;
    //for (let i = 1; i < data.length; i++){
    //    if (highest < data[i].high)
    //        highest = data[i].high;
    //    if (lowest > data[i].low)
    //        lowest = data[i].low;
    //}
  // 参考别人后求最大最小值的简化方式
    var highest = data.reduce(function(x, y){ return x.high > y.high ? x : y }).high;
    var lowest = data.reduce(function(x, y){ return x.low < y.low ? x : y }).low;

    // 计算每个柱形的宽度的一半（避免后面循环时需要除以二）
    var everyHalfWidth = width / data.length / 2;
    // 计算数据中最高点和最低点的差值
    var heightDiff = highest - lowest;

    // 循环绘制每个柱形
    for (let i = 0; i < data.length; i++){
        // 获取某点在Canvas中的Y轴坐标
        var getRealY = function(point){
            return (highest - point) / heightDiff * height;
        };

        let 
            // 获取柱形中心X坐标
            centerX = everyHalfWidth * (i * 2 + 1),
            // 获取最高点Y坐标
            highY = getRealY(data[i].high),
            // 获取最低点Y坐标
            lowY = getRealY(data[i].low),
            // 获取开盘点Y坐标
            openY = getRealY(data[i].open),
            // 获取收盘点Y坐标
            closeY = getRealY(data[i].close);

        // 根据开盘点和收盘点的高低来判断是红色还是蓝色
        if (openY > closeY){
            ctx.fillStyle = '#F65655';
            ctx.strokeStyle = '#F65655';
        } else {
            ctx.fillStyle = '#56F1F2';
            ctx.strokeStyle = '#56F1F2';
        }
        // 绘制柱形
        ctx.fillRect(centerX - everyHalfWidth, openY, everyHalfWidth * 2, closeY - openY);

        // 绘制中间线
        ctx.beginPath();
        ctx.moveTo(centerX, highY);
        ctx.lineTo(centerX, lowY);
        ctx.stroke();
    }
*/
 
```