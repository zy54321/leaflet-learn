# leaflet 入门

leaflet是领先的开源JavaScript库为移动设备设计的互动地图。重33 KB的JS,所有映射大多数开发人员所需要的特性。

leaflet设计简单,性能和可用性。它有效地在所有主要的桌面和移动平台,可以扩展的插件,有一个美丽的,易于使用和证据确凿的API和一个简单的、易读的源代码,是一个快乐作出贡献。

## 一、地图初始化

这里我们创建一个地图在地图的div,添加瓷砖的选择,然后添加一个标记,在弹出一些文本:

 地图在编写任何代码之前,您需要做以下页面上制备步骤:

- 包括leaflet CSS文件标题部分的文档:

```
`<link rel=``"stylesheet"` `href=``"https://unpkg.com/leaflet@1.0.1/dist/leaflet.css"` `/>`
```

　　（没有这个文件可以去下载，也可以自己引入,以下不再累述），[点击下载（稳定版本）](http://cdn.leafletjs.com/leaflet/v1.0.1/leaflet.zip);

- 包括传单JavaScript文件:

```
`<script src=``"https://unpkg.com/leaflet@1.0.1/dist/leaflet.js"``></script>`
```

- 放一个div元素与特定的id,你想要你的地图:

```
`<div id=``"mapid"``></div>`
```

　　（id名字可以随便设定，但是必须与下面js代码定义个一样。。）

- 确保定义的映射容器有一个高度,例如通过设置CSS（必须定义一个高度，因为无法获取指定的id名，因此这个库并没有进行高度的处理设置，自己必须设置高度，如同div默认是没有高度的一样）:

```
`#mapid { height: 180px; }`
```

- 现在您已经准备好初始化地图,用它做一些东西。

## 二、设置地图

让我们创建一个地图的中心北京某个地点漂亮Mapbox街道瓷砖。首先我们将初始化和设置它的视图映射到我们选择的地理坐标和缩放级别（里面的  mapid  必须和设置的id保持一致）:

```
`var` `mymap = L.map(``'mapid'``).setView([39.9788, 116.30226], 14);`
```

　　

在默认情况下(我们没有通过任何选项创建地图实例)时,所有鼠标和触摸交互启用了在地图上,它有放大和归因控制。（这些都可以通过js来控制，目前只是入门，有深入了解的可以自己摸索）

注意setView调用返回地图对象——大多数leaflet方法这样的行为时不返回一个显式的值,它允许方便类jQuery方法控制。

接下来,我们将添加一个砖层来增加我们的地图,在这种情况下这是一个Mapbox街道砖层。创建一个砖层通常涉及瓷砖图像的模板设置URL(在Mapbox得到你),归因的文本和的最大缩放级别层:

```javascript
`L.tileLayer(``'https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}'``, {``    ``attribution: ``'Map data © <a href="http://openstreetmap.org">OpenStreetMap</a> contributors, <a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="http://mapbox.com">Mapbox</a>'``,``    ``maxZoom: 18,``    ``id: ``'your.mapbox.project.id'``,``    ``accessToken: ``'your.mapbox.public.access.token'``}).addTo(mymap);`
```

　　确保所有的代码称为div和传单。js包含。就是这样!现在你有一个工作leaflet地图。

## 三、标记,圆圈和多边形

除了砖层,您可以轻松地向地图添加其他东西,包括标志、折线、多边形、圆和弹出窗口。让我们添加一个标记:

```
`L.marker([39.9788, 116.30226]).addTo(mymap)``    ``.bindPopup(``"北京大厦<br>"``).openPopup();`
```

添加一个圆是相同的(除了指定半径米作为第二个参数),但允许您控制看起来如何通过选择在创建对象时作为最后一个参数:

```
`L.circle([39.9908, 116.26625], 500, {``    ``color: ``'red'``,``    ``fillColor: ``'#f03'``,``    ``fillOpacity: 0.5``}).addTo(mymap).bindPopup(``"颐和园欢迎你"``);`
```

添加一个多边形很简单:

```
`L.polygon([``    ``[39.92096, 116.38591],``    ``[39.91079, 116.38676],``    ``[39.91118, 116.3962],``    ``[39.92014, 116.39482]``]).addTo(mymap).bindPopup(``"故宫"``);`
```

　　

弹出窗口时通常使用您想要一些信息附加到地图上的一个特定的对象。传单有非常方便的快捷方式（和上面添加的方式一样，直接添加或者，另行添加，实际是一样的  openPopup是表示默认是否打开）:

```
`marker.bindPopup(``"北京大厦"``).openPopup();``circle.bindPopup(``"颐和园"``);``polygon.bindPopup(故宫");`
```

　试着点击我们的对象。bindPopup方法高度与指定的HTML内容弹出你的标记弹出出现当你点击对象,和openPopup方法(标记)立即打开弹出。

   您还可以使用弹出窗口层(当你需要更多的东西比附加一个弹出一个对象):

```
`var` `popup = L.popup()``    ``.setLatLng([51.5, -0.09])``    ``.setContent(``"I am a standalone popup."``)``    ``.openOn(mymap);`
```

　　这里我们用openOn代替遭受因为它处理自动关闭之前打开弹出当打开一个新的良好的可用性。

处理事件

每次发生在leaflet,比如用户点击地图上标记或缩放变化,相应的对象发送一个事件,你可以订阅功能。它允许您对用户交互（这里显示的是每次你点击位置的经纬度）:

```
`function` `onMapClick(e) {``    ``alert(``"You clicked the map at "` `+ e.latlng);``}` `mymap.on(``'click'``, onMapClick);`
```

每个对象都有自己的一组事件,有关详细信息,请参阅文档。侦听器函数的第一个参数是一个事件对象,它包含有用的信息的事件发生。例如,地图点击事件对象(e在上面的示例中)latlng属性点击出现的位置。

让我们改善我们的例子中,使用一个弹出一个警告:

```
`var` `popup = L.popup();` `function` `onMapClick(e) {``    ``popup``        ``.setLatLng(e.latlng)``        ``.setContent(``"You clicked the map at "` `+ e.latlng.toString())``        ``.openOn(mymap);``}` `mymap.on(``'click'``, onMapClick);`
```

　　试着点击地图,你会看到一个弹出的坐标。
