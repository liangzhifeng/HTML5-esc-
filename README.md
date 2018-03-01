# Monitor-ESC-event-after-fullscreen
HTML5全屏之后，监听esc按键事件

在网上查了很多资料，网上的提供的几种方法不能解决该问题，大多是回答者没有理解提问者的意图。

# 问题背景

页面上有一个全屏的按钮，一个退出全屏的按钮，在点击全屏按钮的时候，浏览器自带了一个可以点击ESC键退出全屏模式。

相当于要区分全屏、退出全屏、ESC三个事件，前两个本来就有事件监听，问题就变成了要监听ESC事件且要与全屏、退出全屏的事件相区分。

网络上提供了以下几种思路：

# 无效的方法一
    window.onkeydown= function(e){
        let a = 12;
    }
 
onkeydown在正常页面点ESC键是可以触发的,但是在全屏之后再点击是没有反应的，据说是浏览器故意这样的。

# 无效的方法二
    window.onresize = function(){
        if(!checkFull()){
        //要执行的动作
        }
    }

    function checkFull(){
        var isFull =  document.fullscreenEnabled || window.fullScreen || document.webkitIsFullScreen || document.msFullscreenEnabled;

        //to fix : false || undefined == undefined
        if(isFull === undefined) isFull = false;
        return isFull;
    }

该方法在全屏后，点击ESC键的确可以在onresize事件响应，但问题是：点击全屏和退出全屏按钮也进入到这个方法。

# 无效的方法三
      document.addEventListener("fullscreenchange", function () {
        if (document.fullscreenElement != null) {
          console.info("Went full screen");
        } else {
          console.info("Exited full screen");
        }
      });
  
  存在的问题同上.
  
# 正确的方法
以下代码是在Angular2项目中，如果是JQuery也是类似的写法。

          let that = this;
          window.onresize = function(){

             // enter是全屏按钮的ID,that.el.nativeElement.querySelector是Angular2中的写法，等价于jQuery中的$('#enter')
             let fullScreenButton =  that.el.nativeElement.querySelector('#enter');
             if(fullScreenButton == null){
                if(that.isClickFullScreenButton == false){//------------------- 这个地方就是捕捉到了全屏时，点键盘ESC键的事件啦！！！

                  // 这里是全屏后，点击键盘ESC键的业务逻辑
                  dosomething1();
                }
              }

              // isClickFullScreenButton是点击了全屏、退出全屏按钮的标志位，在onresize事件结束的时候置为false,在全屏、退出全屏点击时置为true

              that.isClickFullScreenButton = false;
          };

      
           /** 全屏按钮事件 **/
           fullScreenChange(){

              //isClickFullScreenButton是点击了全屏、退出全屏按钮的标志位，在onresize事件结束的时候置为false,在全屏、退出全屏按钮点击时置为true

              this.isClickFullScreenButton = true;

              let docElm = document.documentElement;
              if (docElm.requestFullscreen) {
                docElm.requestFullscreen();
              }
              else if (docElm.webkitRequestFullScreen) {
                docElm.webkitRequestFullScreen();
              }

              // 全屏按钮的业务逻辑
              dosomething2();
          }

          /** 退出全屏的按钮事件 **/
          fullScreenChangeEXIT(){

              // isClickFullScreenButton是点击了全屏、退出全屏按钮的标志位，在onresize事件结束的时候置为false,在全屏、退出全屏按钮点击时置为true
              this.isClickFullScreenButton = true;

              if (document.exitFullscreen) {
                document.exitFullscreen();
              }
              else if (document.webkitCancelFullScreen) {
                document.webkitCancelFullScreen();
              }

              // 退出全屏按钮的业务逻辑
              dosomething3();
         }
