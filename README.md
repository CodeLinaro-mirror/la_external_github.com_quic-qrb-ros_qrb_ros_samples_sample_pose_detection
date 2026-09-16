<div >
  <h1>Sample Hrnet Pose Estimation</h1>
  <p align="center">
</div>


![](https://github.com/qualcomm-qrb-ros/qrb_ros_samples/blob/gif/ai_vision/sample_hrnet_pose_estimation/resource/result_image.gif)

---

## 👋 Overview

The **sample_hrnet_pose_estimation** sample provides high-precision human pose estimation capabilities.
It processes input images and publishes the following ROS 2 topics:

- **`/pose_estimation_results`**: Output images with visualized pose keypoints.
- **`/pose_estimation_points`**: Raw keypoint coordinates in a structured message format.

For model information, please refer to [HRNetPose - Qualcomm AI Hub](https://aihub.qualcomm.com/iot/models/hrnet_pose?searchTerm=hrnet)

![](./resource/sample_hrnet_pose_pipeline.png)

| Node Name                                                    | Function                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `hrnet_pose_estimation_node` | The node subscribes to /image_raw for image input, performs pose estimation, and publishes the results to /pose_estimation_results and /pose_estimation_points topics. |
| `image_publisher_node` | image_publisher is a ros jazzy packages, can publish image ros topic with local path. For more detail, Please refer to [image_publisher](https://github.com/ros-perception/image_pipeline). |
|`qrb_ros_camera`| The QRB ROS Camera is a ROS package to publish the images from Qualcomm CSI and GMSL cameras. For more detail, Please refer to [QRB ROS CAMERA](https://github.com/qualcomm-qrb-ros/qrb_ros_camera).|
|`qrb_ros_nn_inference`| QRB_ROS_NN_inference is a ROS2 package for performing neural network model, providing AI-based perception for robotics applications. For more detail, Please refer to [QRB ROS NN Inference](https://github.com/qualcomm-qrb-ros/qrb_ros_nn_inference). |


## 🔎 Table of contents

  * [Used ROS Topics](#-used-ros-topics)
  * [Supported targets](#-supported-targets)
  * [Installation](#-installation)
  * [Usage](#-usage)
  * [Build from source](#-build-from-source)
  * [Contributing](#-contributing)
  * [Contributors](#%EF%B8%8F-contributors)
  * [FAQs](#-faqs)
  * [License](#-license)

## ⚓ Used ROS Topics 

| ROS Topic                       | Type                                       | Description                                                  |
| ------------------------------- | ------------------------------------------ | ------------------------------------------------------------ |
| `/image_raw `                   | `sensor_msgs.msg.Image `                   | The sample hrnet pose estimation input image.                |
| `/qrb_inference_input_tensor `  | `qrb_ros_tensor_list_msgs/msg/TensorList ` | The preprocessed image is converted into an input msg for nn inference. |
| `/qrb_inference_output_tensor ` | `qrb_ros_tensor_list_msgs/msg/TensorList ` | Message after nn inference inference                         |
| `/pose_estimation_results`      | `sensor_msgs.msg.Image`                    | Output images with visualized pose keypoints.                |
| `/pose_estimation_points`       | `geometry_msgs.msg.PolygonStamped`         | Raw keypoint coordinates in a structured message format.     |

Note: `/pose_estimation_points` contains the coordinates of 17 key points on the original image. Developers can subscribe to this topic for secondary development. Key points include: nose, left eye, right eye, etc. The specific correspondence is as follows: 

| ID  | Body Part      | ID  | Body Part      |
|-----|----------------|-----|----------------|
| 1   | Nose           | 10  | Right Wrist    |
| 2   | Right Eye      | 11  | Left Wrist     |
| 3   | Left Eye       | 12  | Right Hip      |
| 4   | Right Ear      | 13  | Left Hip       |
| 5   | Left Ear       | 14  | Right Knee     |
| 6   | Right Shoulder | 15  | Left Knee      |
| 7   | Left Shoulder  | 16  | Right Ankle    |
| 8   | Right Elbow    | 17  | Left Ankle     |
| 9   | Left Elbow     |     |                |

## 🎯 Supported targets

<table >
  <tr>
    <th>GMSL Camera Support</th>
    <td>LI-VENUS-OX03F10-OAX40-GM2A-118H(YUV)</td>
  </tr>
</table>



## 🚀 Usage

<details>
  <summary>Usage details</summary>
Run the sample on device

```bash
# setup runtime environment
source /opt/ros/jazzy/setup.bash
export ROS_DOMAIN_ID=124

export ADSP_LIBRARY_PATH="/usr/lib/rfsa/adsp;/usr/lib/rfsa/adsp/hexagon-v81"
export CDSP_LIBRARY_PATH="/vendor/dsp/cdsp0;/usr/lib/rfsa/adsp/hexagon-v81"

# Launch the sample with image publisher, You can replace 'image_path' with the path to your desired image.
ros2 launch sample_hrnet_pose_estimation launch_with_image_publisher.py image_path:=/opt/ros/jazzy/share/sample_hrnet_pose_estimation/input_image.jpg
# Launch the sample with qrb ros camera.
ros2 launch sample_hrnet_pose_estimation launch_with_qrb_ros_camera_iq10.py
```

Open a new terminal and use rqt to view topic `/pose_estimation_results`.

Open a new terminal and run the following command to view the `/pose_estimation_points` topic.

```
source /opt/ros/jazzy/setup.bash
export ROS_DOMAIN_ID=124
ros2 topic echo /pose_estimation_points
```

</details>

## 🤝 Contributing

We love community contributions! Get started by reading our [CONTRIBUTING.md](CONTRIBUTING.md).<br>
Feel free to create an issue for bug report, feature requests or any discussion💡.

## ❤️ Contributors

Thanks to all our contributors who have helped make this project better!

<table>
  <tr>
    <td align="center"><a href="https://github.com/Ceere"><img src="https://avatars.githubusercontent.com/u/45758726?v=4" width="100" height="100" alt="Ceere"/><br /><sub><b>Ceere</b></sub></a></td>
  </tr>
</table>


## ❔ FAQs

<details>
<summary>Can multiple people be detected?</summary><br>
No, if multiple people need to be detected, please segment the images of multiple people into individual ones and perform separate detection.
</details>



## 📜 License

Project is licensed under the [BSD-3-Clause](https://spdx.org/licenses/BSD-3-Clause.html) License. See [LICENSE](../../LICENSE) for the full license text.
