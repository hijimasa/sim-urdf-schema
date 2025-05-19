# URDF Extended Schema

This document summarizes the specifications for extended tags defined in `sim-urdf-schema`.

## 1. `<joint>` Extension

| Tag Name           | Type       | Unit        | Description                           | Default Value |
| ------------- | ------- | --------- | ---------------------------- | ------ |
| `<stiffness>` | `float` | N·m/rad   | Joint spring stiffness. The larger the value, the stiffer the joint.   | 0.0    |
| `<damping>`   | `float` | N·m·s/rad | Joint damping (viscous friction). Specifies the amount of motion decay. | 0.0    |

### Example

```xml
<joint name=“elbow_joint” type=“revolute”>
  <parent link=“upper_arm”/>
  <child  link=“forearm”/>
  <origin xyz=“0 0 0” rpy=“0 0 0”/>
  <axis xyz=“0 0 1”/>
  <!-- Extended tags -->
  <stiffness>50.0</stiffness>
  <damping>0.5</damping>
</joint>
```

## 2. `<simulation>` container

Define a `<simulation>` block at the root of the URDF or directly under `<link>` or `<joint>` to describe sensor and simulator-specific settings.

```xml
<simulation>
  <!-- Place multiple sensor definitions -->
</simulation>
```

### 2.1 `<sensor>` tag common elements

| Element name             | Type        | Unit | Description        | Default value |
| --------------- | -------- | -- | --------- | ------ |
| `<update_rate>` | `float`  | Hz | Sensor update rate | 30.0   |
| `<topicName>`   | `string` | —  | Output topic name   | —      |
| `<frameName>`   | `string` | —  | TF frame name  | —      |

### 2.2 Elements for LiDAR

When `type=“lidar”`, the following `<ray>` block is required.

* `<ray>`
  * `<scan>`
    * `<horizontal>`
      * `<samples>`: `int` ― Number of samples in the horizontal direction
      * `<resolution>`: `int` ― Binning resolution between samples
      * `<min_angle>`: `float` (rad) ― Scan start angle
      * `<max_angle>`: `float` (rad) ― Scan end angle
    * `<range>`
      * `<min>`: `float` (m) ― Minimum detection distance
      * `<max>`: `float` (m) ― Maximum detection distance
    * `<resolution>`: `float` (m) ― Distance resolution
    * `<noise>`
      * `<type>`: `string` ― Noise type (e.g., “gaussian”)
      * `<mean>`: `float`
      * `<stddev>`: `float`

### 2.3 Camera/depth camera elements

Used when `type=“camera”` or `type=“depth_camera”`.
* `<horizontal_fov>`: `float` (rad) ― Horizontal field of view
* `<image>`
  * `<width>`: `int` (px)
  * `<height>`: `int` (px)
* `<clip>`
  * `<near>`: `float` (m)
  * `<far>`: `float` (m)
* `<cameraName>`: `string`
* `<imageTopicName>`: `string`
* `<cameraInfoTopicName>`: `string`

### 2.4 Example

```xml
<simulation>

  <sensor name=“lidar_link” type=“lidar”>
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

  <sensor name=“camera_link” type=“camera”>
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

  <sensor name=“depth_camera_link” type=“depth_camera”>
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

## 3. `<collision_material>` tag

This tag is used to define friction characteristics within the `<collision>` block of the link.

| Tag name                             | Type        | Description                                                   | Default value |
| ------------------------------- | -------- | ---------------------------------------------------- | ------ |
| `<collision_material>` (name attribute) | `string` | Material name                                                  | —      |
| `<friction>`                    | —        | Specifies the static/dynamic friction coefficient within `<collision_material>`. Has the following attributes. | —      |
| - `static`                      | `float`  | Static friction coefficient                                               | 0.0    |
| - `dynamic`                     | `float`  | Dynamic friction coefficient                                               | 0.0    |

### Definition example

```xml
<collision_material name=“wheel”>
  <friction static=“1.0” dynamic=“1.0”/>
</collision_material>
```

### Example of application to URDF

```xml
<link name=“sample_link”>
  <collision>
    <collision_material name=“wheel”/>
  </collision>
</link>
```

## 4. Schema validation

Syntax checking of extended tags is possible using XML Schema Definition (XSD).

```bash
xmllint --noout --schema urdf-extended.xsd your_model.urdf
```
