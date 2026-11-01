<h1 align='center'>Autonomous Vehicle(AV) Self Driving System</h1>

<div align='center'>

![Nix Flake Check](https://github.com/DivitMittal/CARLA-Autonomous-Driving/actions/workflows/flake-check.yml/badge.svg)
![Nix Flake Lock Update](https://github.com/DivitMittal/CARLA-Autonomous-Driving/actions/workflows/flake-lock-update.yml/badge.svg)

</div>

<div align='center'>
    <img src='./assets/simulation_preview.png' alt='Preview of the autonomous-driving simulation' title='Simulation'/>
</div>

---

- **Enhance Perception Systems**: Utilize deep learning techniques for semantic segmentation to improve the vehicle's understanding of its surroundings.
- **Integrate Multi-Modal Sensors**: Combine data from RGBA cameras and LiDAR sensors to achieve comprehensive environmental awareness.
- **Facilitate Research and Development**: Provide a modular and extensible codebase that supports the development of novel autonomous driving algorithms.
- **Ensure Realism and Accuracy**: Leverage the high-fidelity simulation capabilities of CARLA to mimic real-world driving conditions and challenges.

## Features

- **Realistic Simulation**: Leverage the high-fidelity CARLA simulator to create diverse driving scenarios.
- **Autonomous Driving Algorithms**: Implemented state-of-the-art algorithms for perception, planning, and control.
- **Sensor Integration**: Support for various sensors including cameras, LiDAR, radar, and GPS.
- **Modular Architecture**: Easily extend and customize components to fit your research or development needs.
- **Data Collection**: Tools for collecting and analyzing simulation data for training and evaluation.
- **Visualization**: Real-time visualization of vehicle dynamics, sensor data, and decision-making processes.

## Deep Learning for Semantic Segmentation

This project leverages advanced deep learning methodologies to perform semantic segmentation, enhancing the vehicle's perception capabilities by accurately identifying and classifying various elements within the driving environment. The integration of RGBA sensor data and LiDAR ensures a robust and precise understanding of the surroundings.

### Core Components:

- **RGBA Sensors**: Capture rich color and texture information, aiding in the distinction of objects, lane markings, and environmental features.
- **LiDAR Integration**: Provides detailed depth and distance information, complementing RGBA data to achieve a comprehensive 3D perception of the environment.
- **Neural Network Architecture**: Utilizes CNNs tailored for real-time semantic segmentation, ensuring efficient and accurate processing of sensor data.
- **Training Pipeline**: Includes scripts and tools for training models on simulated data, allowing for extensive experimentation and optimization.
- **Evaluation Metrics**: Implements robust evaluation frameworks to assess the performance and reliability of the semantic segmentation models under various scenarios.

### Benefits:

- **Enhanced Accuracy**: Combining RGBA and LiDAR data improves the precision of object detection and classification.
- **Real-Time Processing**: Optimized neural network architectures ensure that semantic segmentation can be performed in real-time, essential for autonomous driving applications.
- **Scalability**: The modular design allows for easy integration of additional sensors or alternative deep learning models as needed.

### Technologies/Software/Non-Standard libraries used:

| Technology | Application                            |
| ---------- | -------------------------------------- |
| CARLA      | Vehicle Simulator                      |
| OpenCV     | For data visualization from RGB Camera |
| Keras      | For deep-learning CNN model            |
| Tensorflow | Optimizing the model weights           |
| Pygame     | Manual Control                         |

## Getting Started

### Prerequisites

- **CARLA simulator** (0.9.x). Download a release from the [CARLA releases page](https://github.com/carla-simulator/carla/releases) and extract it anywhere.
- **Python** at a version supported by your CARLA release (the `carla` PyPI wheel must match the simulator version you installed).
- A GPU is strongly recommended for both the simulator and model training.

### 1. Install the Python dependencies

From the repository root:

```bash
python -m venv .venv
```

```bash
# Windows
.venv\Scripts\activate
# Linux / macOS
source .venv/bin/activate
```

```bash
pip install pygame numpy carla tensorflow matplotlib opencv-python
```

Pin `carla` to the same version as your simulator, e.g. `pip install carla==0.9.15`. The list of packages also lives in [`src/requirements.txt`](./src/requirements.txt).

> **Nix users:** `nix develop` (or `direnv allow`) enters a devshell with the project's formatters and git hooks. It does **not** provide the Python packages above; install those with `pip` as shown.

### 2. Start the CARLA server

Launch the simulator from wherever you extracted it. It listens on `127.0.0.1:2000` by default, which is what the scripts expect.

```bash
# Windows
CarlaUE4.exe
```

```bash
# Linux
./CarlaUE4.sh
```

Every script accepts `--host` and `--port` if your server runs elsewhere.

### 3. Run the scripts

The scripts use package-relative imports, so run them as modules **from the repository root** with `python -m src.<script>`.

#### Multi-sensor grid demo

Spawns a Tesla Model 3 fitted with four RGB cameras, a LiDAR and a semantic LiDAR, and shows all feeds in a 2×3 Pygame grid.

```bash
python -m src.basic_environment_with_sensors
```

Press **Esc** or **Q** to quit. Useful options: `--res 1920x1080`, `--async` (default is synchronous mode).

#### Manual control

Drives a vehicle that is **already spawned** in the simulator (it looks for a `*model3*` blueprint, so start the sensor demo above in another terminal first, or spawn a vehicle yourself).

```bash
python -m src.control_window
```

| Key              | Action          |
| ---------------- | --------------- |
| **W** / **↑**    | Throttle        |
| **S** / **↓**    | Brake           |
| **A** / **←**    | Steer left      |
| **D** / **→**    | Steer right     |
| **Space**        | Hand brake      |

Close the window to stop. Use `--vehicle-filter PATTERN` to control a different vehicle.

#### CNN self-steering (autonomous lane following)

Spawns a vehicle on a road in **Town05**, feeds the front camera through the trained CNN and steers from its predicted angle. The script needs a trained model (see the next section) and defaults to `./model/lane_model`.

```bash
python -m src.model_self_steer --town Town05
```

Press **Q** in the camera window to stop. Pass `--model PATH` to point at a model stored elsewhere.

### 4. Train the lane model (optional)

The two notebooks in `src/` cover data exploration and model training:

- [`carlaCNN.ipynb`](./src/carlaCNN.ipynb): connects to a running CARLA server, loads Town05, spawns traffic and streams camera footage. A good place to see the sensors in action.
- [`model_build.ipynb`](./src/model_build.ipynb): trains the steering CNN on labelled lane images and saves it with `model.save(...)`.

```bash
pip install jupyter
```

```bash
jupyter notebook src/model_build.ipynb
```

The training notebook reads PNG images from `archive/train_label/` relative to the notebook, where each filename ends in the steering angle (e.g. `..._frame_0_label.png`). It saves the trained model to `model/lane_model` **next to the notebook**, i.e. `src/model/lane_model`. Either move that directory to the repository root or run the self-steering script with:

```bash
python -m src.model_self_steer --town Town05 --model src/model/lane_model
```

---

### TL;DR [Video Presentation](https://drive.google.com/drive/folders/1te0HDyyQaOI47RANxhuFZEUii8KMuGsd?usp=share_link)
