# 🪐 3D Asset Rendering Script

![266879371-69064f78-a752-40d6-bd36-ea7c15ffa1ec](https://github.com/allenai/objaverse-xl/assets/28768645/41edb8e3-d2f6-4299-a7f8-418f1ef28029)

Scripts for rendering 3D assets with [Blender](https://www.blender.org/).
Rendering is the process of taking pictures of the 3D objects.
These images can then be used for caption generation, multi-modal asset retrieval, or training AI models.

## 🖥️ Setup

1. Clone the repository and enter the rendering directory:

```bash
git clone https://github.com/yunusskeete/blender-python-rendering.git && \
  cd blender-python-rendering/scripts/rendering
```

2. Download Blender:

```bash
wget https://download.blender.org/release/Blender4.1/blender-4.1.0-linux-x64.tar.xz && \
  tar -xf blender-4.1.0-linux-x64.tar.xz && \
  rm blender-4.1.0-linux-x64.tar.xz
```

3. If you're on a headless Linux server, install Xorg and start it:

```bash
sudo apt-get install xserver-xorg -y && \
  sudo python3 start_x_server.py start
```

4. Install the Python dependencies. Note that Python >3.8 is required:

```bash
cd ../.. && \
  pip install -r requirements.txt && \
  pip install -e . && \
  cd scripts/rendering
```

## 📸 Usage

### 🐥 Minimal Example

After setup, we can start to render objects using the `blender_script.py` script from the `./scripts/rendering/` directory:

```bash
python blender_script.py --object_path path/to/object.usda --num_renders 12 --output_dir path/to/output_folder
```

By If we look in that directory, we'll find the following files:

```bash
> cd path/to/output_folder
> ls
000.npy  001.npy  002.npy  003.npy  004.npy  005.npy  006.npy  007.npy  008.npy  009.npy  010.npy  011.npy  metadata.json
000.png  001.png  002.png  003.png  004.png  005.png  006.png  007.png  008.png  009.png  010.png  011.png
```

Here, we see that there are 12 renders `[000-011].png`.
Each render will look something like one of the 4 images shown below, but likely with the camera at a different location as its location is randomized during rendering:

![266900773-69d79e26-4df1-4bd2-854c-7d3c888adae7](https://github.com/allenai/objaverse-xl/assets/28768645/440fe28e-4c5a-4460-a4df-6730028c0b22)

Additionally, there are 12 npy files `[000-011].npy`, which include information about the camera's pose for a given render.
We can read the npy files using:

```python
import numpy as np
array = np.load("000.npy")
```

where array is now a 3x4 [camera matrix](https://en.wikipedia.org/wiki/Camera_matrix) that looks something like:

```python
array([[6.07966840e-01,  7.93962419e-01,  3.18103019e-08,  2.10451518e-07],
       [4.75670159e-01, -3.64238620e-01,  8.00667346e-01, -5.96046448e-08],
       [6.35699809e-01, -4.86779213e-01, -5.99109232e-01, -1.66008198e+00]])
```

Finally, we also have a `metadata.json` file, which contains metadata about the object and scene:

```json
{
  "animation_count": 0,
  "armature_count": 0,
  "edge_count": 2492,
  "file_size": 108916,
  "lamp_count": 1,
  "linked_files": [],
  "material_count": 0,
  "mesh_count": 3,
  "missing_textures": {
    "count": 0,
    "file_path_to_color": {},
    "files": []
  },
  "object_count": 8,
  "poly_count": 984,
  "random_color": null,
  "scene_size": {
    "bbox_max": [
      4.999998569488525,
      6.0,
      1.0
    ],
    "bbox_min": [
      -4.999995231628418,
      -6.0,
      -1.0
    ]
  },
  "shape_key_count": 0,
  "vert_count": 2032
}
```

### 🎛 Configuration

<!-- Inside of `main.py` there is a `render_objects` function that provides various parameters allowing for customization during the rendering process:

- `render_dir: str = "~/.objaverse"`: The directory where the objects will be rendered. Default is `"~/.objaverse"`.
- `download_dir: Optional[str] = None`: The directory where the objects will be downloaded. Thanks to fsspec, we support writing files to many file systems. To use it, simply use the prefix of your filesystem before the path. For example hdfs://, s3://, http://, gcs://, or ssh://. Some of these file systems require installing an additional package (for example s3fs for s3, gcsfs for gcs, fsspec/sshfs for ssh). Start [here](https://github.com/rom1504/img2dataset#file-system-support) for more details on fsspec. If None, the objects will be deleted after they are rendered.
- `num_renders: int = 12`: The number of renders to save for each object. Defaults to `12`.
- `processes: Optional[int] = None`: The number of processes to utilize for downloading the objects. If left as `None` (default), it will default to `multiprocessing.cpu_count() * 3`.
- `save_repo_format: Optional[Literal["zip", "tar", "tar.gz", "files"]] = None`: If specified, the GitHub repository will be deleted post rendering each object from it. Available options are `"zip"`, `"tar"`, and `"tar.gz"`. If `None` (default), no action is taken.
- `only_northern_hemisphere: bool = False`: If `True`, only the northern hemisphere of the object is rendered. This is useful for objects acquired via photogrammetry, as the southern hemisphere may have holes. Default is `False`.
- `render_timeout: int = 300`: Maximum number of seconds to await completion of the rendering job. Default is `300` seconds.
- `gpu_devices: Optional[Union[int, List[int]]] = None`: Specifies GPU device(s) for rendering. If an `int`, the GPU device is randomly chosen from `0` to `gpu_devices - 1`. If a `List[int]`, a GPU device is randomly chosen from the list. If `0`, the CPU is used. If `None` (default), all available GPUs are utilized.

To tweak the objects that you want to render, go into `main.py` and update the `get_example_objects` function. -->
TBD.

#### Example

To render objects using a custom configuration:

<!-- ```bash
python3 main.py --render_dir s3://bucket/render/directory --num_renders 15 --only_northern_hemisphere True
``` -->
TBD.

## To-Do
- [ ] Test all file types
- [ ] Fix camera positioning occluding part of object
- [ ] Add Logging
- [ ] Containerise
