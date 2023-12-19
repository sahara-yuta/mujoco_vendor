# mujoco_vendor
mujocoをrosのライブラリとして使用できるようにオーバーラップしたパッケージ。
wstool等でこのリポジトリをクローンし、`catkin build mujoco_vendor`することで、使えるようになる。

## rosrun
以下のように、mujocoに付属するサンプルやシミュレータも実行できる。
```bash
$ rosrun mujoco_vendor basic /path/to/your_mjcf.xml
$ rosrun mujoco_vendor simulate
```