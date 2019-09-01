页面性能类:
1、资源压缩合并, 减少http 请求.
2、非核心代码异步加载.
3、利用浏览器缓存. => 协商缓存 => 强制缓存.
4、使用cdn.
5、预解析DNS.


自己的理解:

浏览器渲染的过程总结:
1、HTML Parse.
2、Style Sheets.
3、Dom tree + Style Rules 合并到 Attachment
4、Render Tree 
5、Layout
6、Painting.
7、Display.

性能优化自己的总结.
性能优化是一个系统性的工程,涉及到前后端网络整个链路。
对前端来讲，我觉得可以分为两类:

1、针对CPU.
   浏览器的渲染触发的reflow,repainter 非常的消耗性能.

   优化的点就是:
    1、减少reflow,repainter.
       如果需要创建多个DOM节点，可以使用DocumentFragment创建完后一次性的加入document.
       浏览器渲染的原理,关键路径优化:

    2、时间分片,不阻塞进程.

2、针对IO.
   减少网络传输,压缩文件,懒加载,
   浏览器缓存.

优点:
    1、suspense.
    
 chrome 调试功能:
 在移动设备上使用开发者工具.在IOS上开始调试.
 https://www.html.cn/doc/chrome-devtools/device-mode/
 https://github.com/CN-Chrome-DevTools/CN-Chrome-DevTools/blob/master/md/Reference/shortcuts.md

 关键路径渲染:
 调试工具分析:
 https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/timeline-tool
 https://www.jianshu.com/p/4da0f0bda768
 60fps.









 




