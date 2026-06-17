Place the two pretrained weight files in THIS folder so the tutorial's
Downloads page can serve them. They are tracked by the Sphinx {download} role.

Required files (exact names used by the Downloads page):

  predictEF.pt            <- EF model      (R(2+1)D-18)
  predictsegmentation.pt  <- Segmentation  (DeepLabV3-ResNet50)

Each file is named after the script that loads it.

After dropping the two files here, rebuild the docs:

  conda activate Sphinx_text
  cd d:\sphinx\sphinx\sphinx
  sphinx-build -b html source build/html

The download links on the Downloads page will then resolve.
