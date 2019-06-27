# 安卓二维码扫描
16340128-lianghw001

这次系分项目我分配到了安卓部分，我们项目实现的是客户通过使用我们的app，可以扫描商家提供的二维码，实现手机点单。
安卓端第一个要实现的就是二维码扫描，我一开始以为只是简单的调用一个api就完事了，没想到还挺麻烦。

- 1.本来想使用官方的控件，在网络上查询后，我发现二维码扫描竟然没有官方实现。平时使用很多的app都有二维码扫描，没想到【二维码扫描】这个功能实际上没有官方实现。
- 2.进一步查询后我得知二维码扫描主要使用zxing和zbar两个库，zbar库不再维护，而zxing库不能简单import，需要自己引入jar包。我使用了一个进行了简化的库。
- 3.由于二维码扫描需要动用摄像头，涉及隐私，需要动态申请权限（本文章不讨论）。

## 后面是二维码扫描的具体实现（注意以下部分不涉及权限）

- 1.在build.gradle(project)中，添加源
  ```
  allprojects{
    repositories{
        google()
        jcenter()
        maven {url 'https://jitpack.io'}
        
    }
  }
  ```
- 2.在build.gradle(app)中，添加dependencies
  ```
  dependencies {
    implementation 'com.github.yuzhiqiang1993:zxing:2.2.8''
  }
  ```
- 3.在二维码扫描页面中，添加initQR()，自行设计功能
  ```
  Intent initQR(){
    Intent intent = new Intent(QRcodeScanActivity.this, CaptureActivity.class);
    ZxingConfig config = new ZxingConfig();
    config.setShowFlashLight(false);//不显示闪光灯按钮
    config.setShowAlbum(false);//不显示闪光灯按钮
    config.setPlayBeep(false);//不播放扫描声音
    config.setShake(true);//震动
    config.setDecodeBarCode(false);//不扫描条形码
    config.setReactColor(R.color.colorAccent);//设置扫描框四个角的颜色
    config.setFrameLineColor(R.color.colorAccent);//设置扫描框边框颜色
    config.setScanLineColor(R.color.colorAccent);//设置扫描线的颜色
    config.setFullScreenScan(false);//不全屏扫描
    intent.putExtra(Constant.INTENT_ZXING_CONFIG, config);
    return intent;
  }
  ```
- 4.在二维码扫描页面中，添加点击事件，点击开始扫描
  ```
  ib_scan.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
      Intent intent = initQR();
      startActivityForResult(intent, REQUEST_CODE_SCAN);
    }
  })
  ```
- 5.在二维码扫描页面中，onActivityResult，接受扫描二维码返回的数据
  ```
  @Override
  protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    // 扫描二维码/条码回传
    if (requestCode == REQUEST_CODE_SCAN && resultCode == RESULT_OK) {
      if (data != null) {
        String data = data.getStringExtra(Constant.CODED_CONTENT);
      }
    }
  }
  ```

