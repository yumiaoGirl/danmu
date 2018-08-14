# 弹幕
微信小程序实现输入弹幕显示的功能并播放音乐
1.使用方法：微信开发者工具打开项目，换上自己的小程序APPId，既可运行
2.代码实现：
    1）代码中有默认的弹幕弹出，输入框中输入弹幕后会直接显示，是因为暂时存在的缓存中，可以换成掉接口的方式
      stopMusicFlag:false,//控制音乐播放
      danArr: ["Hi 你好吗", "我可以每天都能看见你嘛？", "我想我们能永远在一起", "往后余生都是你", "往事匆匆", "你总是会感动"]//弹幕字幕
      doommList.push(new Doomm(txt, Math.ceil(Math.random() * 100), Math.ceil(Math.random() * 30), getRandomColor()));
      this.setData({
          doommData: doommList
      });
      var doommList = [];
      var i = 0;//用做唯一的wx:key
      class Doomm {
          constructor(text, top, time, color) {
              this.text = text;
              this.top = top;
              this.time = time;
              this.color = color;
              this.display = true;
              let that = this;
              this.id = i++;
              setTimeout(function () {
                  doommList.splice(doommList.indexOf(that), 1);//动画完成，从列表中移除这项
                  page.setData({
                      doommData: doommList
                  })
              }, this.time * 1000)//定时器动画完成后执行。
          }
      }
      function getRandomColor() {
          let rgb = []
          for (let i = 0; i < 3; ++i) {
              let color = Math.floor(Math.random() * 256).toString(16)
              color = color.length == 1 ? '0' + color : color
              rgb.push(color)
          }
          return '#' + rgb.join('')
      }
      //以上参考网上的代码加入了个人思想，如有侵权，请联系我删除，大佬对不起
    2）播放音乐：目前小程序中只支持远程地址和图片，所以我把音乐传到了我男朋友的服务器上
      wx.playBackgroundAudio({
            dataUrl: 'https://www.imchenlei.com/wanghouyusheng.mp3',
            title: '往后余生',
        });
        //这个音乐默认是一打开就播放的，我加了点击事件，可以点击暂停，并且在页面销毁时关闭音乐
        /**
         * 当页面销毁时调用
         */
        onUnload: function () {
            wx.stopBackgroundAudio()
        },
        stopMusic: function(){//停止播放
            let that = this;
            if (that.data.stopMusicFlag){
                that.playMusic();
            }else{
                that.setData({
                    stopMusicFlag : true
                })
                wx.pauseBackgroundAudio();
                that.stopRefresh();
            }
        }
    3）旋转音乐小图标：
        // 创建动画
        var animation = wx.createAnimation({
            duration: 2000,
            timingFunction: "linear"
        })
        this.animation = animation;
 //以上均为本人所写，如有问题请联系我，如要转发请注明出处，谢谢。
       
        
