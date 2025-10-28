<div align="center">

<p align="center">
  <img src="./docs/penguine.png" alt="readme-ai-streamlit" width="100%">
</p>

<p align="center">
  <em>Designed for simplicity, customization, and developer productivity.</em>
</p>
# 🐍 Bramy

📖 **Overview**
Bramy is a highly efficient and modular Python-based project designed for integrating various computer vision techniques. This repo provides a flexible interface for handling computer vision tasks, tracking, and control systems, primarily aimed at autonomous driving technology.

---

✨ **Features**
- **Modular Camera Interface:** Easily work with different camera drivers.
- **Object Tracking:** Utilizes advanced sorting algorithms for object tracking.
- **Post-Processing Tools:** Includes features for data and image processing.
- **ROS2 Integration:** Seamlessly integrates with ROS2 for communication in robotic systems.

---

🚀 **Installation**
To get started with Bramy:
```bash
# Clone the repository
git clone https://github.com/v0ntr3n/bramy.git
cd bramy

# Install dependencies
pip install -r requirements.txt  # Ensure this file exists

# Setup the package
python setup.py install
```

---

🛠️ **Usage**
After installation, you can launch the Bramy system using the ROS launch file:
```bash
ros2 launch rclpy/car/launch/bramy.launch.py
```

You can integrate your own camera by modifying the `astra_camera.py` under `rclpy/car/bramy/camera`.

---

📦 **Technologies**
- **Python**: The primary programming language used across modules.
- **ROS2**: Framework for writing robot software with communication tools.
- **Kalman Filter**: Utilized for predictive filtering in tracking.

---

🔧 **Configuration**
While the default configuration provided is set for development, you can tailor your settings in `rclpy/car/launch/bramy.launch.py` file to point to your specific camera or control parameters.

---

✅ **Requirements**
- **Python 3.6+**
- **ROS2 Foxy**
- **CMake** - For compiling some C++ extensions if applicable.

---

🤝 **Contributing**
We welcome contributions! Please fork the repository and make your changes in a separate branch. Once your changes are ready, submit a pull request. Refer to the [CONTRIBUTING.md](#) for more detailed guidelines.

---

📄 **Documentation**
More comprehensive documentation is available in the `docs` directory. Start with `docs/index.md`.

---

❤️ **Acknowledgements**
Thanks to all contributors and especially the creators of the ROS2 framework for providing such a robust platform.

---

📝 **Changelog**
- **[2025-10-28]**: Updated README documentation
- **[2025-06-05]**: Multiple system replacements and optimizations
- **First Commit**: Project initialization

---

🗂️ **Repository Structure**
```
bramy/
│
├── LICENSE                  # License information (Apache-2.0)
├── README.md                # You're reading this now!
│
├── docs/                    # Documentation resources
│   └── penguine.png         # Example image file
│
└── rclpy/                   # ROS-based codebase
    └── car/
        ├── bramy/           # Main module for computer vision
        │   ├── __init__.py
        │   ├── camera/
        │   │   └── astra_camera.py
        │   ├── ocsort/
        │   │   ├── association.py
        │   │   ├── kalmanfilter.py
        │   │   └── ocsort.py
        ├── launch/          # Launch configurations
        │   └── bramy.launch.py
        ├── setup.py         # Setup script
        └── test/            # Tests for the module
            ├── test_copyright.py
            ├── test_flake8.py
            └── test_pep257.py
```

---

This README is designed to help users and contributors understand and navigate the Bramy project effectively. For more advanced usage, refer to the specific Python scripts and ROS launch files.
