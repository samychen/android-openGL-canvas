# OpenGL canvas

[中文版README](https://github.com/ChillingVan/android-openGL-canvas/blob/master/README-zh.md) 

Idea from: 
* Codes of Android source code under package com.android.gallery3d.glrenderer
* [GPUImage](https://github.com/CyberAgent/android-gpuimage)
* [grafika](https://github.com/google/grafika)

## Features
* The canvasGL provides similar API as android canvas. And there are GLViews that can be extended to custom your own view to draw things with OpenGL.
* Similar to the filters of GPUImage, you can apply the filter to the bitmap draw into the GLViews. 
* Provides GLViews that using GLSurfaceView and TextureView. 
* The GLContinuousView can provide high performance continuous rendering animation because it uses openGL to draw in its own thread.

## Requirements
* Android API14 or higher (OpenGL ES 2.0)

## Usage

### Gradle dependency

```groovy
repositories {
    jcenter()
    maven { url "https://jitpack.io" }
}

dependencies {
    compile 'com.github.ChillingVan:android-openGL-canvas:v1.1.0'
}
```

### Sample Code

* Custom your view.
```java
public class MyGLView extends GLView {

    public MyGLView(Context context) {
        super(context);
    }

    public MyGLView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }


    @Override
    protected void onGLDraw(ICanvasGL canvas) {
        // draw things with canvas here
    }
}
```


![canvas](https://github.com/ChillingVan/android-openGL-canvas/raw/master/screenshots/canvas-example.png)

* The Usage of GLContinuouslyView, GLTextureView, GLContinuousTextureView, GLSurfaceTextureProducerView and GLSharedContextView is similar.


* Using canvas to draw
```java
        canvas.drawBitmap(textBitmap, left, top);
        
        // transform
        canvas.save();
        canvas.rotate(rotateDegree, x, y);
        canvas.drawBitmap(bitmap, left, top);
        canvas.restore();
        // or
        CanvasGL.BitmapMatrix matrix = new CanvasGL.BitmapMatrix();
        matrix.postScale(2.1f, 2.1f);
        matrix.postRotate(90);
        canvas.drawBitmap(bitmap, matrix);
        
        // apply filter to the bitmap
        textureFilter = new ContrastFilter(2.8f);
        canvas.drawBitmap(bitmap, left, top, textureFilter);
```

![filters](https://github.com/ChillingVan/android-openGL-canvas/raw/master/screenshots/filter_example.png)


* And can be used with camera, just run the camera sample code on a real device(not in emulator) to see what will happen.

![camera](https://github.com/ChillingVan/android-openGL-canvas/raw/master/screenshots/camera-example.jpg)


* If you do not want to use GLView, you can use OffScreenCanvas to draw things and fetch it by getDrawingBitmap.

See the wiki page for more use case.[here](https://github.com/ChillingVan/android-openGL-canvas/wiki)

## Notice
* The onGLDraw method in GLView runs in its own thread but not the main thread. 
* I haven't implemented all the filters in GPUImage. I will add more later. If you need, you can take my code as example to implement your filter. It is simple.
* Remember to call onResume and onPause in the Activity lifecycle when using GLContinuousView and GLContinuousTextureView.

## License
    Copyright 2016 ChillingVan.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
