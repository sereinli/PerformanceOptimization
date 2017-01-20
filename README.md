# PerformanceOptimization
Android 性能优化
#1、布局优化
    A.使用抽象布局标签
      ① <include></include>  主要是布局xml的复用
      ② <merge></merge>       可以减少布局嵌套，减少解析和onMeasure消耗用SurfaceView或TextureView代替普通View</br>
      ③ <ViewStub></ViewStub>  很少出现的布局或者条件不同显示不同样式的需求可以采用，不占用资源</br>
    
    B.去掉不必要的嵌套和View节点
      在使用了include后可能导致布局嵌套过多，多余不必要的layout节点，从而导致解析变慢。可以使用布局调优工具hierarchy viewer查看</br>
      ① 首次不使用的节点设置为GONE或者使用ViewStub
      ② RelativeLayout替代LinearLayout
    
    C.减少不必要的inflate，ViewItem的复用等
  
    D.其它性能提升的方式
      ① 用SurfaceView或TextureView代替普通View。SurfaceView或TextureView可以通过将绘图操作移动到另一个单独线程上提高性能。普通View的绘制过程都是在主线程(UI线程)中完成，如果某些绘图操作影响性能就不好优化了，这时我们可以考虑使用SurfaceView和TextureView，他们的绘图操作发生在UI线程之外的另一个线程上。因为SurfaceView在常规视图系统之外，所以无法像常规试图一样移动、缩放或旋转一个SurfaceView。TextureView是Android4.0引入的，除了与SurfaceView一样在单独线程绘制外，还可以像常规视图一样被改变。
      ② 使用RenderJavascript。RenderScript是Adnroid3.0引进的用来在Android上写高性能代码的一种语言，语法给予C语言的C99标准，他的结构是独立的，所以不需要为不同的CPU或者GPU定制代码代码。
      例如，对图片颜色进行反转，一般的做法是将图片解析出Bitmap，然后遍历每个像素，进行反转，图片分辨率越大，耗时越长。如果使用RenderScript进行反转的话，时间将大大缩短。下面是我测试一张图反转的两个方法对比，时间上差异明显。
      代码地址：[Render Script图片反转Demo](https://github.com/sereinli/RenderScriptRevertImage.git)
