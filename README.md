# vue-jsBridge
vue项目中使用jsBridge技术 和原生APP通信

android [jsBridge库](https://github.com/lzyzsd/JsBridge)
ios [jsBridge库](https://github.com/marcuswestin/WebViewJavascriptBridge)

###在vue中使用插件
```
  import Vue from 'vue'
  import JsBridge from 'jsBridge-plugin'
  
  Vue.use(JsBridge);
```

###在vue中使用
```
  import {setJsBridge} from 'jsBridge-plugin'  //引入插件

  //vue生命周期钩子
  ready : function(){ 
      
      //注册js方法 让原生调用
      setJsBridge((bridge)=>{
          
          bridge.registerHandler(this.$jsBridgeCmd.publish, (data, responseCallback)=>{
              $("#app_resp").html("点击了提交按钮");
              this.publish().then((id)=>{
                  responseCallback(id);
              })
          });
          
      });
  }
  
  methods: {
        //点击事件方法或者其他场景
        addImage(){
            //调用原生方法  需要原生那边也注册方法
            this.$jsBridge.callHandler(
                this.$jsBridgeCmd.chooseImage 
                , { name : '添加图片' }
                , (responseData) =>{
                    //回调函数
                }
            );
        },
  }
  
  
```
