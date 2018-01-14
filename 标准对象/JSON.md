用浏览器访问Yahoo的天气API，查看返回的JSON数据，然后返回城市、气温预报等信息：
```
'use strict'

var url = 'https://query.yahooapis.com/v1/public/yql?q=select%20*%20from%20weather.forecast%20where%20woeid%20%3D%202151330&format=json';
// 从远程地址获取JSON:
$.getJSON(url, function (data) {
    // 获取结果:
    var city = data.query.results.channel.location.city;
    var forecast = data.query.results.channel.item.forecast;
    var result = {
        city: city,
        forecast: forecast
    };
    alert(JSON.stringify(result, null, '  '));
    
});
```