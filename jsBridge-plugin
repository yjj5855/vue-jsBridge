'use strict'
import device from './device'

var JsBridgePlugin = {};

JsBridgePlugin.install = function (Vue, option) {

    //初始化jsBridge
    Vue.prototype.$jsBridge = {
        registerHandler : function (name,callback) {
            console.error('没有注册成功')
        },
        callHandler : function (name, params, callback) {
            console.error('没有调用成功')
        }
    }
    //所有原生和js通信的命令
    Vue.prototype.$jsBridgeCmd = {
        chooseImage : 'chooseImage',
    }

    //先注释掉 初始化jsBridge 需要用到时再初始化
    if(device.webView){
        if(device.android){
            androidSetWebViewJavascriptBridge(function(bridge) {

                Vue.prototype.$jsBridge = bridge;

                console.log('初始化$jsBridge'+ (typeof bridge.registerHandler == 'function'))

            })
        }else if(device.ios){
            iosSetupWebViewJavascriptBridge(function(bridge) {
                Vue.prototype.$jsBridge = bridge

                // console.log('初始化$jsBridge',Vue.prototype.$jsBridge)
            })
        }
    }
    
};
export default JsBridgePlugin;

//安卓
export function androidSetWebViewJavascriptBridge(callback) {
    if (window.WebViewJavascriptBridge) {
        console.log('有WebViewJavascriptBridge对象')
        return callback(WebViewJavascriptBridge)
    } else {
        console.log('没有WebViewJavascriptBridge对象,等待WebViewJavascriptBridgeReady事件回调')
        document.addEventListener(
            'WebViewJavascriptBridgeReady'
            , function(){
                console.log('WebViewJavascriptBridgeReady事件回调了')
                callback(WebViewJavascriptBridge)
            },
            false
        );
    }
}

//ios
export function iosSetupWebViewJavascriptBridge(callback) {
    if (window.WebViewJavascriptBridge) { return callback(WebViewJavascriptBridge); }
    if (window.WVJBCallbacks) { return window.WVJBCallbacks.push(callback); }
    window.WVJBCallbacks = [callback];
    var WVJBIframe = document.createElement('iframe');
    WVJBIframe.style.display = 'none';
    WVJBIframe.src = 'wvjbscheme://__BRIDGE_LOADED__';
    document.documentElement.appendChild(WVJBIframe);
    setTimeout(function() { document.documentElement.removeChild(WVJBIframe) }, 0)
}

let func = function () {

};
if(device.webView) {
    if (device.android) {
        func =  androidSetWebViewJavascriptBridge
    } else if (device.ios) {
        func =  iosSetupWebViewJavascriptBridge
    }
}

//在路由中使用这个方法 注册web方法让原生调用
export let setJsBridge = func;
