# URDF Extended Schema

このドキュメントは `sim-urdf-schema` で定義する拡張タグの仕様をまとめたものです。

## 1. `<joint>` 拡張

| タグ名           | 型       | 単位        | 説明                           | デフォルト値 |
| ------------- | ------- | --------- | ---------------------------- | ------ |
| `<stiffness>` | `float` | N·m/rad   | 関節のばね剛性。大きいほど硬いジョイントになります。   | 0.0    |
| `<damping>`   | `float` | N·m·s/rad | 関節のダンピング（粘性摩擦）。運動の減衰量を指定します。 | 0.0    |

### 使用例

```xml
<joint name="elbow_joint" type="revolute">
  <parent link="upper_arm"/>
  <child  link="forearm"/>
  <origin xyz="0 0 0" rpy="0 0 0"/>
  <axis xyz="0 0 1"/>

  <!-- 拡張タグ -->
  <stiffness>50.0</stiffness>
  <damping>0.5</damping>
</joint>
```

## 2. `<simulation>` コンテナ

URDF のルートまたは `<link>`・`<joint>` の直下に `<simulation>` ブロックを定義し、センサやシミュレータ固有設定を記述します。

```xml
<simulation>
  <!-- センサ定義を複数配置 -->
</simulation>
```

### 2.1 `<sensor>` タグ 共通要素

| 要素名             | 型        | 単位 | 説明        | デフォルト値 |
| --------------- | -------- | -- | --------- | ------ |
| `<update_rate>` | `float`  | Hz | センサの更新レート | 30.0   |
| `<topicName>`   | `string` | —  | 出力トピック名   | —      |
| `<frameName>`   | `string` | —  | TF フレーム名  | —      |

### 2.2 LiDAR 用要素

`type="lidar"` の場合、以下の `<ray>` ブロックを必須で含みます。

* `<ray>`

  * `<scan>`

    * `<horizontal>`

      * `<samples>`: `int` ― 水平方向のサンプル数
      * `<resolution>`: `int` ― サンプル間のビニング解像度
      * `<min_angle>`: `float` (rad) ― スキャン開始角度
      * `<max_angle>`: `float` (rad) ― スキャン終了角度
  * `<range>`

    * `<min>`: `float` (m) ― 最小検出距離
    * `<max>`: `float` (m) ― 最大検出距離
    * `<resolution>`: `float` (m) ― 距離解像度
  * `<noise>`

    * `<type>`: `string` ― ノイズタイプ（例: "gaussian"）
    * `<mean>`: `float`
    * `<stddev>`: `float`

### 2.3 カメラ／深度カメラ 用要素

`type="camera"` または `type="depth_camera"` の場合に使用します。

* `<horizontal_fov>`: `float` (rad) ― 水平視野角
* `<image>`

  * `<width>`: `int` (px)
  * `<height>`: `int` (px)
* `<clip>`

  * `<near>`: `float` (m)
  * `<far>`: `float` (m)
* `<cameraName>`: `string`
* `<imageTopicName>`: `string`
* `<cameraInfoTopicName>`: `string`

### 2.4 使用例

```xml
<simulation>
  <sensor name="lidar_link" type="lidar">
    <update_rate>10</update_rate>
    <ray>
      <scan>
        <horizontal>
          <samples>400</samples>
          <resolution>1</resolution>
          <min_angle>-3.1415</min_angle>
          <max_angle>3.1415</max_angle>
        </horizontal>
      </scan>
      <range>
        <min>0.05</min>
        <max>20.0</max>
        <resolution>0.01</resolution>
      </range>
      <noise>
        <type>gaussian</type>
        <mean>0.0</mean>
        <stddev>0.01</stddev>
      </noise>
    </ray>
    <topicName>/lidar/scan</topicName>
    <frameName>lidar_link</frameName>
  </sensor>

  <sensor name="camera_link" type="camera">
    <update_rate>30.0</update_rate>
    <horizontal_fov>1.3962634</horizontal_fov>
    <image>
      <width>800</width>
      <height>600</height>
    </image>
    <clip>
      <near>0.02</near>
      <far>300</far>
    </clip>
    <cameraName>/camera</cameraName>
    <imageTopicName>image_raw</imageTopicName>
    <cameraInfoTopicName>camera_info</cameraInfoTopicName>
    <frameName>camera_link</frameName>
  </sensor>

  <sensor name="depth_camera_link" type="depth_camera">
    <update_rate>10.0</update_rate>
    <horizontal_fov>1.3962634</horizontal_fov>
    <image>
      <width>320</width>
      <height>240</height>
    </image>
    <clip>
      <near>0.02</near>
      <far>300</far>
    </clip>
    <cameraName>/depth_camera</cameraName>
    <imageTopicName>depth_image_raw</imageTopicName>
    <cameraInfoTopicName>depth_camera_info</cameraInfoTopicName>
    <frameName>depth_camera_link</frameName>
  </sensor>
</simulation>
```

## 3. `<collision_material>` タグ

リンクの `<collision>` ブロック内で摩擦特性を定義するためのタグです。

| タグ名                             | 型        | 説明                                                   | デフォルト値 |
| ------------------------------- | -------- | ---------------------------------------------------- | ------ |
| `<collision_material>` (name属性) | `string` | 素材名                                                  | —      |
| `<friction>`                    | —        | `<collision_material>`内で静的/動的摩擦係数を指定する要素。以下の属性を持ちます。 | —      |
| - `static`                      | `float`  | 静的摩擦係数                                               | 0.0    |
| - `dynamic`                     | `float`  | 動的摩擦係数                                               | 0.0    |

### 定義例

```xml
<collision_material name="wheel">
  <friction static="1.0" dynamic="1.0"/>
</collision_material>
```

### URDF への適用例

```xml
<link name="sample_link">
  <collision>
    <collision_material name="wheel"/>
  </collision>
</link>
```

## 4. スキーマ検証

XML Schema Definition (XSD) を用いて、拡張タグの構文チェックが可能です。

```bash
xmllint --noout --schema urdf-extended.xsd your_model.urdf
```
