## 兼容IE的rgba属性

	filter:progid:DXImageTransform.Microsoft.Gradient(startColorStr=#AARRGGBB, endColorStr=#AARRGGBB)

这个滤镜主要是用作渐变的，颜色值组成依次是16进制的不透明度, Red, Green, Blue；这里只需要半透明，起始颜色和终止颜色一致即可因为 IE9 同时支持 rgba 和 filter，所以为防止不透明度为双倍，需要对其进行处理，比如用 `:root .rgba70{}` 来重设 IE9 的 filter。不透明度的计算用 JavaScript 的话公式为：假定小数表示的不透明度为 a，则 16 进制版的是 `Math.floor(a * 255).toString(16)`

**完整代码**

```css
.rgba70{
background: rgba(0,0,0, 0.7);
	filter:progid:DXImageTransform.Microsoft.Gradient(startColorStr=#B2000000, endColorStr=#B2000000);
}
:root .rgba70{
	filter:none;
}
```

**javascript计算**

```javascript
var convertBtn = document.getElementById('convert'),                    
opacity = document.getElementById('opacity'),
opacityHex = document.getElementById('opacityHex');
function getHexOpacity(opacity) {
    return Math.floor(parseFloat(opacity, 10) * 255).toString(16).toUpperCase();
}
function calc() {
    opacityHex.innerHTML = getHexOpacity(opacity.value);
}
convertBtn.onclick = calc;
opacity.onkeyup = function(e) {
    e = e || window.event;
    if (e.keyCode == 13) {
        calc();
    }
};
```