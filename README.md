# HTML5-esc-
HTML5全屏之后，监听esc按键事件

在网上查了很多资料，没有找到解决办法。

      window.onresize = function(){
         let fullScreenButton =  that.el.nativeElement.querySelector('#enter');
          if(fullScreenButton == null){
            if(that.isClickFullScreenButton == false){
              that.fullScreenB=false;
            }
          }
          that.isClickFullScreenButton = false;
      };
      
      
      // 全屏按钮事件
  fullScreenChange(){
    this.isClickFullScreenButton = true;
    this.fullScreenB=true;
 
    var docElm = document.documentElement;
    if (docElm.requestFullscreen) {
      docElm.requestFullscreen();
    }
    else if (docElm.webkitRequestFullScreen) {
      docElm.webkitRequestFullScreen();
    }
  }
  
  // 退出全屏的按钮事件
  fullScreenChangeEXIT(){
    this.isClickFullScreenButton = true;
    this.fullScreenB=false;

    if (document.exitFullscreen) {
      document.exitFullscreen();
    }
    else if (document.webkitCancelFullScreen) {
      document.webkitCancelFullScreen();
    }
  }
