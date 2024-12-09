# mujoco_vendor
![Build Status](https://sahara-yuta.github.io/mujoco_vendor/build.svg)
![File Generation Status](https://sahara-yuta.github.io/mujoco_vendor/file_generated.svg)

mujocoをrosのライブラリとして使用できるようにオーバーラップしたパッケージ。
wstool等でこのリポジトリをクローンし、`catkin build mujoco`することで、使えるようになる。

## 公式シミュレータ・サンプルプログラムの実行
以下のように、mujocoに付属するシミュレータやサンプルも実行できる。
```bash
$ rosrun mujoco basic /path/to/your_mjcf.xml
$ rosrun mujoco simulate
```

## 公式モデル
mujocoリポジトリのmodelフォルダーをdevel/shareにコピーしているため、`catkin_find`などにより利用できる。
```bash
$ rosrun mujoco simulate `catkin_find mujoco model/humanoid/humanoid.xml`
```

## ライブラリとしての利用方法
一般的なパッケージと同様に、package.xmlに
```xml
<depend>mujoco</depend>
```
CMakeLists.txtに
```cmake
find_package(catkin REQUIRED COMPONENT mujoco)
catkin_package()
include_directories(${catkin_INCLUDE_DIRS})
link_libraries(${catkin_LIBRARIES})
```
などを追記することで、C++のパッケージにリンクすることができる。
ヘッダファイルは以下のように`mujoco/*.h`としてインクルードできる。
```c++
#include <mujoco/mujoco.h>
```