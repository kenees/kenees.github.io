---
title: 微信小程序中动态更改svg颜色
date: 2022-10-24 16:40:08
tag:
    - Taro
    - 微信小程序
    - svg

categories:
  - 前端
---

同样的一个图片， 在应用中选中与未选中颜色不一样， 特别是微信小程序中，为了节省空间， 打算使用svg， 可使用js动态更改其配色。
本篇文章采用的技术栈为 Taro构建的跨端小程序开发。

### 1. 自定义一个Svg组件

```javascript
    import { Component } from 'react'
    import { Image } from '@tarojs/components'
    const Base64 = require("../../utils/base64");

    export default class Svg extends Component{

        state = {
            svgStyle: "",
        }

        componentWillMount() {
            const { src, color } = this.props; 
            if (src) {
                if (src.indexOf('base64') > -1) { // 
                    const svgBase64 = src.substring(src.indexOf(",") + 1, src.length);
                    const svg = Base64.decode(svgBase64);
                    if (/<svg /.test(svg)) {
                        let newSvg;
                        if (/fill=".*?"/.test(svg)) {
                            newSvg = svg.replace(/fill=".*?"/, `fill="${color}"`);  // SVG有默认色
                        } else {
                            newSvg = svg.replace(/<svg /, `<svg fill="${color}"`); // 无默认色
                        }
                        this.setState({
                            svgStyle: 'data:image/svg+xml;base64,' + Base64.encode(newSvg)
                        })
                    }
                } else {
                    wx.getFileSystemManager().readFile({
                        filePath: src,
                        encoding:"base64",
                        success: res => {
                            const svg = Base64.decode(res?.data);
                            if (/<svg /.test(svg)) {
                                let newSvg;
                                if (/fill=".*?"/.test(svg)) {
                                    newSvg = svg.replace(/fill=".*?"/, `fill="${color}"`);  // SVG有默认色
                                } else {
                                    newSvg = svg.replace(/<svg /, `<svg fill="${color}"`); // 无默认色
                                }
                                this.setState({
                                    svgStyle: 'data:image/svg+xml;base64,' + Base64.encode(newSvg)
                                })
                            }
                        }
                    })
                }
            }
        }


        render() {
            return (
                <>
                    <Image src={this.state.svgStyle} style={{fill: 'red'}} />
                </>
            )
        }
    }
```

### 2. 其它位置使用

```javascript
    import Svg from '@/components/Svg';

    <Svg src="https://xxx.svg" color="red" />
```

### 3. 相关基础库

```javascript
var Base64 = {
    // private property
    _keyStr: "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=",
    // public method for encoding
    encode: function(input) {
      var output = "";
      var chr1, chr2, chr3, enc1, enc2, enc3, enc4;
      var i = 0;
      input = Base64._utf8_encode(input);
  
      while (i < input.length) {
  
        chr1 = input.charCodeAt(i++);
        chr2 = input.charCodeAt(i++);
        chr3 = input.charCodeAt(i++);
  
        enc1 = chr1 >> 2;
        enc2 = ((chr1 & 3) << 4) | (chr2 >> 4);
        enc3 = ((chr2 & 15) << 2) | (chr3 >> 6);
        enc4 = chr3 & 63;
  
        if (isNaN(chr2)) {
          enc3 = enc4 = 64;
        } else if (isNaN(chr3)) {
          enc4 = 64;
        }
  
        output = output + this._keyStr.charAt(enc1) + this._keyStr.charAt(enc2) + this._keyStr.charAt(enc3) + this._keyStr.charAt(enc4);
  
      }
  
      return output;
    },
  
    // public method for decoding
    decode: function(input) {
      var output = "";
      var chr1, chr2, chr3;
      var enc1, enc2, enc3, enc4;
      var i = 0;
  
      input = input.replace(/[^A-Za-z0-9\+\/\=]/g, "");
  
      while (i < input.length) {
  
        enc1 = this._keyStr.indexOf(input.charAt(i++));
        enc2 = this._keyStr.indexOf(input.charAt(i++));
        enc3 = this._keyStr.indexOf(input.charAt(i++));
        enc4 = this._keyStr.indexOf(input.charAt(i++));
  
        chr1 = (enc1 << 2) | (enc2 >> 4);
        chr2 = ((enc2 & 15) << 4) | (enc3 >> 2);
        chr3 = ((enc3 & 3) << 6) | enc4;
  
        output = output + String.fromCharCode(chr1);
  
        if (enc3 != 64) {
          output = output + String.fromCharCode(chr2);
        }
        if (enc4 != 64) {
          output = output + String.fromCharCode(chr3);
        }
  
      }
  
      output = Base64._utf8_decode(output);
  
      return output;
    },
  
    // private method for UTF-8 encoding
    _utf8_encode: function(string) {
      string = string.replace(/\r\n/g, "\n");
      var utftext = "";
  
      for (var n = 0; n < string.length; n++) {
  
        var c = string.charCodeAt(n);
  
        if (c < 128) {
          utftext += String.fromCharCode(c);
        } else if ((c > 127) && (c < 2048)) {
          utftext += String.fromCharCode((c >> 6) | 192);
          utftext += String.fromCharCode((c & 63) | 128);
        } else {
          utftext += String.fromCharCode((c >> 12) | 224);
          utftext += String.fromCharCode(((c >> 6) & 63) | 128);
          utftext += String.fromCharCode((c & 63) | 128);
        }
  
      }
  
      return utftext;
    },
  
    // private method for UTF-8 decoding
    _utf8_decode: function(utftext) {
      var string = "";
      var i = 0;
      var c = 0;
      var c1 = 0;
      var c2 = 0;
  
      while (i < utftext.length) {
  
        c = utftext.charCodeAt(i);
  
        if (c < 128) {
          string += String.fromCharCode(c);
          i++;
        } else if ((c > 191) && (c < 224)) {
          c2 = utftext.charCodeAt(i + 1);
          string += String.fromCharCode(((c & 31) << 6) | (c2 & 63));
          i += 2;
        } else {
          c2 = utftext.charCodeAt(i + 1);
          c3 = utftext.charCodeAt(i + 2);
          string += String.fromCharCode(((c & 15) << 12) | ((c2 & 63) << 6) | (c3 & 63));
          i += 3;
        }
  
      }
  
      return string;
    }
  }
  
  module.exports = Base64
```
