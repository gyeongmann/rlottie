# rlottie

![BlockNote image](https://camo.githubusercontent.com/9453e1782bb56a07788396ebfdfaaad016330556108796a75267a11a2fa5f966/68747470733a2f2f63692e6170707665796f722e636f6d2f6170692f70726f6a656374732f7374617475732f6e33786f6e786b316f6f6f3673376e723f7376673d747275652670617373696e67546578743d77696e646f77732532302d25323070617373696e67)![BlockNote image](https://camo.githubusercontent.com/b91cd6307fb68bfb8c742f255ddcb5da6c994f386fdac931e23f53b7d14ff13a/68747470733a2f2f6261646765732e6769747465722e696d2f724c6f747469652d6465762f636f6d6d756e6974792e737667)

<br />

![BlockNote image](https://github.com/Samsung/rlottie/raw/master/.Gifs/logo.png)

rlottie is a platform independent standalone c++ library for rendering vector based animations and art in realtime.

Lottie loads and renders animations and vectors exported in the bodymovin JSON format. Bodymovin JSON can be created and exported from After Effects with [bodymovin](https://github.com/bodymovin/bodymovin), Sketch with [Lottie Sketch Export](https://github.com/buba447/Lottie-Sketch-Export), and from [Haiku](https://www.haiku.ai/).

For the first time, designers can create and ship beautiful animations without an engineer painstakingly recreating it by hand. Since the animation is backed by JSON they are extremely small in size but can be large in complexity!

Here are small samples of the power of Lottie.

| ![1.gif](<https://withme.s3.amazonaws.com/interImg/0f4eaf24-c149-4c15-ac06-88eacf5d3b8d_1.gif>) | ![2.gif](<https://withme.s3.amazonaws.com/interImg/f3b2b274-15b3-4990-9fa2-832d3ab75c00_2.gif>) |
| ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |

![Examples1.gif](https://withme.s3.amazonaws.com/interImg/70c76f84-8c06-4393-b603-6b06d12a40df_Examples1.gif)

## Contents

*   [Building Lottie](https://github.com/gyeongmann/rlottie/edit/master/README.md#building-lottie)

    *   [Meson Build](https://github.com/gyeongmann/rlottie/edit/master/README.md#meson-build)
    *   [Cmake Build](https://github.com/gyeongmann/rlottie/edit/master/README.md#cmake-build)
    *   [Test](https://github.com/gyeongmann/rlottie/edit/master/README.md#test)

*   [Demo](https://github.com/gyeongmann/rlottie/edit/master/README.md#demo)

*   [Previewing Lottie JSON Files](https://github.com/gyeongmann/rlottie/edit/master/README.md#previewing-lottie-json-files)

*   [Quick Start](https://github.com/gyeongmann/rlottie/edit/master/README.md#quick-start)

*   [Dynamic Property](https://github.com/gyeongmann/rlottie/edit/master/README.md#dynamic-property)

*   [Supported After Effects Features](https://github.com/gyeongmann/rlottie/edit/master/README.md#supported-after-effects-features)

*   [Issues or Feature Requests?](https://github.com/gyeongmann/rlottie/edit/master/README.md#issues-or-feature-requests)

## Building Lottie

rlottie supports [meson](https://mesonbuild.com/) and [cmake](https://cmake.org/) build system. rlottie is written in `C++14`. and has a public header dependency of `C++11`

### Meson Build

install [meson](http://mesonbuild.com/Getting-meson.html) and [ninja](https://ninja-build.org/) if not already installed.

Run meson to configure rlottie

```javascript
meson build
```

Run ninja to build rlottie

```javascript
ninja -C build
```

### Cmake Build

Install [cmake](https://cmake.org/download/) if not already installed

Create a build directory for out of source `build`

```javascript
mkdir build
```

Run cmake command inside `build` directory to configure rlottie.

```javascript
cd build
cmake ..

# install in a different path. eg ~/test/usr/lib
cmake -DCMAKE_INSTALL_PREFIX=~/test ..

# static build
cmake -DBUILD_SHARED_LIBS=OFF ..
```

Run make to build rlottie

```javascript
make -j 2
```

To install rlottie library

```javascript
make install
```

### Test

Configure to build test

```javascript
meson configure -Dtest=true
```

Build test suit

```javascript
ninja
```

Run test suit

```javascript
ninja test
```

[Back to contents](https://github.com/gyeongmann/rlottie/edit/master/README.md#contents)

## Demo

If you want to see rlottie library in action without building it please visit [rlottie online viewer](http://rlottie.com/)

While building rlottie library it generates a simple lottie to GIF converter which can be used to convert lottie json file to GIF file.

Run Demo

```javascript
lottie2gif [lottie file name]
```

#

## Previewing Lottie JSON Files

Please visit [rlottie online viewer](http://rlottie.com/)

[rlottie online viewer](http://rlottie.com/) uses rlottie wasm library to render the resource locally in your browser. To test your JSON resource drag and drop it to the browser window.

#

## Quick Start

Lottie loads and renders animations and vectors exported in the bodymovin JSON format. Bodymovin JSON can be created and exported from After Effects with [bodymovin](https://github.com/bodymovin/bodymovin), Sketch with [Lottie Sketch Export](https://github.com/buba447/Lottie-Sketch-Export), and from [Haiku](https://www.haiku.ai/).

You can quickly load a Lottie animation with:

```javascript
auto animation = rlottie::Animation::loadFromFile("absolute_path/test.json");
```

You can load a lottie animation from raw data with:

```javascript
auto animation = rlottie::Animation::loadFromData(std::string(rawData), std::string(cacheKey));
```

Properties like `frameRate` , `totalFrame` , `duration` can be queried with:

```javascript
# get the frame rate of the resource.
double frameRate = animation->frameRate();

#get total frame that exists in the resource
size_t totalFrame = animation->totalFrame();

#get total animation duration in sec for the resource
double duration = animation->duration();
```

Render a particular frame in a surface buffer `immediately` with:

```javascript
rlottie::Surface surface(buffer, width , height , stride);
animation->renderSync(frameNo, surface);
```

Render a particular frame in a surface buffer `asyncronousely` with:

```javascript
rlottie::Surface surface(buffer, width , height , stride);
# give a render request
std::future<rlottie::Surface> handle = animation->render(frameNo, surface);
...
#when the render data is needed
rlottie::Surface surface = handle.get();
```

[Back to contents](https://github.com/gyeongmann/rlottie/edit/master/README.md#contents)

## Dynamic Property

You can update properties dynamically at runtime. This can be used for a variety of purposes such as:

*   Theming (day and night or arbitrary themes).
*   Responding to events such as an error or a success.
*   Animating a single part of an animation in response to an event.
*   Responding to view sizes or other values not known at design time.

### Understanding After Effects

To understand how to change animation properties in Lottie, you should first understand how animation properties are stored in Lottie. Animation properties are stored in a data tree that mimics the information hierarchy of After Effects. In After Effects a Composition is a collection of Layers that each have their own timelines. Layer objects have string names, and their contents can be an image, shape layers, fills, strokes, or just about anything that is drawable. Each object in After Effects has a name. Lottie can find these objects and properties by their name using a KeyPath.

### Usage

To update a property at runtime, you need 3 things:

1.  KeyPath
2.  rLottie::Property
3.  setValue()

### KeyPath

A KeyPath is used to target a specific content or a set of contents that will be updated. A KeyPath is specified by a list of strings that correspond to the hierarchy of After Effects contents in the original animation. KeyPaths can include the specific name of the contents or wildcards:

*   Wildcard *

    *   Wildcards match any single content name in its position in the keypath.

*   Globstar **

    *   Globstars match zero or more layers.

### Properties

`rLottie::Property` is an enumeration of properties that can be set. They correspond to the animatable value in After Effects and the available properties are listed below.

```javascript
enum class Property {
    FillColor,     /*!< Color property of Fill object , value type is rlottie::Color */
    FillOpacity,   /*!< Opacity property of Fill object , value type is float [ 0 .. 100] */
    StrokeColor,   /*!< Color property of Stroke object , value type is rlottie::Color */
    StrokeOpacity, /*!< Opacity property of Stroke object , value type is float [ 0 .. 100] */
    StrokeWidth,   /*!< stroke with property of Stroke object , value type is float */
    ...
};
```

### setValue()

`setValue()` requires a keypath of string and value. The value can be `Color`, `Size` and `Point` structure or a function that returns them. `Color`, `Size`, and `Point` vary depending on the type of `rLottie::Property`. This value or function(callback) is called and applied to every frame. This value can be set differently for each frame by using the `FrameInfo` argument passed to the function.

### Usage

```javascript
animation->setValue<rlottie::Property::FillColor>("**",rlottie::Color(0, 1, 0));
```

```javascript
animation->setValue<rlottie::Property::FillColor>("Layer1.Box 1.Fill1",
    [](const rlottie::FrameInfo& info) {
         if (info.curFrame() < 15 )
             return rlottie::Color(0, 1, 0);
         else {
             return rlottie::Color(1, 0, 0);
         }
     });
```

[Back to contents](https://github.com/gyeongmann/rlottie/edit/master/README.md#contents)

#

#

## Supported After Effects Features

| **Shapes**                       | **Supported** |
| -------------------------------- | ------------- |
| Shape                            | 👍            |
| Ellipse                          | 👍            |
| Rectangle                        | 👍            |
| Rounded Rectangle                | 👍            |
| Polystar                         | 👍            |
| Group                            | 👍            |
| Trim Path (individually)         | 👍            |
| Trim Path (simultaneously)       | 👍            |
| **Renderable**                   | **Supported** |
| Fill                             | 👍            |
| Stroke                           | 👍            |
| Radial Gradient                  | 👍            |
| Linear Gradient                  | 👍            |
| Gradient Stroke                  | 👍            |
| **Transforms**                   | **Supported** |
| Position                         | 👍            |
| Position (separated X/Y)         | 👍            |
| Scale                            | 👍            |
| Skew                             | ⛔️            |
| Rotation                         | 👍            |
| Anchor Point                     | 👍            |
| Opacity                          | 👍            |
| Parenting                        | 👍            |
| Auto Orient                      | 👍            |
| **Interpolation**                | **Supported** |
| Linear Interpolation             | 👍            |
| Bezier Interpolation             | 👍            |
| Hold Interpolation               | 👍            |
| Spatial Bezier Interpolation     | 👍            |
| Rove Across Time                 | 👍            |
| **Masks**                        | **Supported** |
| Mask Path                        | 👍            |
| Mask Opacity                     | 👍            |
| Add                              | 👍            |
| Subtract                         | 👍            |
| Intersect                        | 👍            |
| Lighten                          | ⛔️            |
| Darken                           | ⛔️            |
| Difference                       | ⛔️            |
| Expansion                        | ⛔️            |
| Feather                          | ⛔️            |
| **Mattes**                       | **Supported** |
| Alpha Matte                      | 👍            |
| Alpha Inverted Matte             | 👍            |
| Luma Matte                       | 👍            |
| Luma Inverted Matte              | 👍            |
| **Merge Paths**                  | **Supported** |
| Merge                            | ⛔️            |
| Add                              | ⛔️            |
| Subtract                         | ⛔️            |
| Intersect                        | ⛔️            |
| Exclude Intersection             | ⛔️            |
| **Layer Effects**                | **Supported** |
| Fill                             | ⛔️            |
| Stroke                           | ⛔️            |
| Tint                             | ⛔️            |
| Tritone                          | ⛔️            |
| Levels Individual Controls       | ⛔️            |
| **Text**                         | **Supported** |
| Glyphs                           | ⛔️            |
| Fonts                            | ⛔️            |
| Transform                        | ⛔️            |
| Fill                             | ⛔️            |
| Stroke                           | ⛔️            |
| Tracking                         | ⛔️            |
| Anchor point grouping            | ⛔️            |
| Text Path                        | ⛔️            |
| Per-character 3D                 | ⛔️            |
| Range selector (Units)           | ⛔️            |
| Range selector (Based on)        | ⛔️            |
| Range selector (Amount)          | ⛔️            |
| Range selector (Shape)           | ⛔️            |
| Range selector (Ease High)       | ⛔️            |
| Range selector (Ease Low)        | ⛔️            |
| Range selector (Randomize order) | ⛔️            |
| expression selector              | ⛔️            |
| **Other**                        | **Supported** |
| Expressions                      | ⛔️            |
| Images                           | 👍            |
| Precomps                         | 👍            |
| Time Stretch                     | 👍            |
| Time remap                       | 👍            |
| Markers                          | 👍            |

#

[Back to contents](https://github.com/gyeongmann/rlottie/edit/master/README.md#contents)

## Issues or Feature Requests?

File github issues for anything that is broken. Be sure to check the [list of supported features](https://github.com/gyeongmann/rlottie/edit/master/README.md#supported-after-effects-features) before submitting. If an animation is not working, please attach the After Effects file to your issue. Debugging without the original can be very difficult. For immidiate assistant or support please reach us in [Gitter](https://gitter.im/rLottie-dev/community#)
