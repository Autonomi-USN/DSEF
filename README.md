<h1 align="center">
    <img alt="DSEF" ttle="DSEF" src="https://raw.githubusercontent.com/Autonomi-USN/DSEF/refs/heads/main/assets/dsef_logo.png?s=200&v=4" />
    <p>Direct Step Edge Follower</p>
</h1>

![ci build](https://github.com/Autonomi-USN/DSEF/actions/workflows/pypi-publish.yml/badge.svg)
![Python Version](https://img.shields.io/pypi/pyversions/dsef)
![Downloads](https://img.shields.io/pypi/dm/dsef)
![PyPI - License](https://img.shields.io/pypi/l/dsef)
[![PyPI version](https://badge.fury.io/py/dsef.svg)](https://badge.fury.io/py/dsef)

# DSEF Package

The [Direct Step Edge Follower (DSEF)](https://www.researchgate.net/publication/390370984_Direct_Step_Edge_Follower_a_novel_edge_follower_algorithm_applied_to_solar_panels_inspections_with_Unmanned_Aerial_Vehicles) is a edge-following algorithm designed for high-precision edge detection with low computational cost. It employs stepwise directional refinement and kernel-based statistical testing to enhance accuracy, particularly in challenging lighting conditions.

## Citation

To cite DSEF in your research, please cite as:

```bibtex
@article{Sivertsen2025DSEF,
  author    = {Agnar Sivertsen and Fabio A. A. Andrade and Marcos Moura and Carlos A. M. Correia and Mariane R. Petraglia},
  title     = {Direct Step Edge Follower: a novel edge follower algorithm applied to solar panels inspections with Unmanned Aerial Vehicles},
  journal   = {Preprint},
  year      = {2025},
  month     = {April},
  url       = {https://www.researchgate.net/publication/390370984_Direct_Step_Edge_Follower_a_novel_edge_follower_algorithm_applied_to_solar_panels_inspections_with_Unmanned_Aerial_Vehicles}
}

```

## How to use

Simple Implementation of DSEF Code to search for the closest line in an example image. Full example can be seen [here](https://github.com/Autonomi-USN/DSEF/blob/main/docs/Simple-DSEF.ipynb).

**Imports**

```python
import sys
import os

import cv2
import matplotlib.pyplot as plt

import dsef
```

**Load Image**

```python
image_path = 'imgs/input_image.jpg'
if not os.path.isfile(image_path):
    raise FileNotFoundError(f"Image '{image_path}' not found.")

img = cv2.imread(image_path)

# Convert the image from BGR (OpenCV default) to RGB (matplotlib default)
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

# Plot the image using matplotlib
plt.imshow(img_rgb)
plt.axis('off')  # Hide the axes
plt.show()
```

![png](https://raw.githubusercontent.com/Autonomi-USN/DSEF/refs/heads/main/docs/readme_imgs/img1.png)

**Initiate DSEF Class**

```python
initial_direction_deg = 270     # The initial direction in degrees
dir_span = 90                   # The span of directions for search

start_pix = (0+200, 0+200)      # Starting pixel (x, y)
end_pix = (1920, 1080)          # Ending pixel (x, y)
speed = "medium"                # Speed of the algorithm, "high", "medium", or "low"
debug = True

dsef_ = dsef.Dsef(initial_direction_deg, dir_span, start_pix, end_pix, speed, debug)
```

**Edge Search**

The dots in the image below represents the steps of the algorithm

```python
## Perform edge search
edge_found, img = dsef_.edge_search(img)

if edge_found:
    print(f"Edge found: {edge_found}")

# Convert the image from BGR (OpenCV default) to RGB (matplotlib default)
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

# Plot the image using matplotlib
plt.imshow(img_rgb)
plt.axis('off')  # Hide the axes
plt.show()
```

    [DEBUG] Overriding d_theta to: 4.0
    [DEBUG] Distance between start and end: 1932.0455481173315
    [DEBUG] Search step: 32.200759135288855
    [DEBUG] Follower step: 22.029071700822982
    [DEBUG] Edge found. Breaking EdgeSearch.
    Edge found: True

![png](https://raw.githubusercontent.com/Autonomi-USN/DSEF/refs/heads/main/docs/readme_imgs/img2.png)

**Edge Follow**

The dots parallel to the edge of the rectangle shows the following path of DSEF algorithm

```python
found_edge_line = None
if edge_found:
    # if the search found an edge, follow it
    found_edge_line, image = dsef_.edge_follow(img)

print(f"Found edge line [(x1,y1),(x2,y2)]: {found_edge_line}")

# Convert the image from BGR (OpenCV default) to RGB (matplotlib default)
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

# Plot the image using matplotlib
plt.imshow(img_rgb)
plt.axis('off')  # Hide the axes
plt.show()
```

    [DEBUG] Distance between start and end: 1932.0455481173315
    [DEBUG] Search step: 32.200759135288855
    [DEBUG] Follower step: 22.029071700822982
    [DEBUG] EdgeFollow message: CANCEL. WE LOST THE EDGE
    Found edge line [(x1,y1),(x2,y2)]: [(522, 380), (-17152.38437246058, 3496.470832276853)]

![png](https://raw.githubusercontent.com/Autonomi-USN/DSEF/refs/heads/main/docs/readme_imgs/img3.png)
