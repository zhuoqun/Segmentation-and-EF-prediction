# Downloads

Everything you need to follow this tutorial is collected here: the three Python
scripts, **links** to the pretrained model weights, and a small sample dataset to
test on. Arrange them in the project layout below, then set the path block at the
top of each script.

---

## Scripts

Right-click → *Save link as…*, or just click to download.

| File | Purpose | Chapter |
|------|---------|---------|
| {download}`PredictEF.py <../downloads/PredictEF.py>` | Ejection-fraction prediction | [EF Prediction](06_ejection_fraction) |
| {download}`PredictSegmentation.py <../downloads/PredictSegmentation.py>` | LV segmentation (videos + `.npy`) | [Segmentation](05_segmentation) |
| {download}`CalculateStats.py <../downloads/CalculateStats.py>` | Optional `MEAN`/`STD` recomputation | [Normalization Statistics](04_normalization_stats) |

Put all three in a `scripts/` folder and configure the path block at the top of
each one.

---

## Pretrained weights

Both prediction scripts load a checkpoint into `WEIGHTS`. Each file is **larger
than 100 MB**, so it is not stored in this repository — download the two
checkpoints from the original
[EchoNet-Dynamic project](https://echonet.github.io/dynamic/) and save them into
`EchoNet-dynamic/stats/` under the names below:

| File | Model | Loaded by | Source |
|------|-------|-----------|--------|
| `predictEF.pt` (~239 MB) | R(2+1)D-18 (EF regression) | `PredictEF.py` | [EchoNet-Dynamic](https://echonet.github.io/dynamic/) |
| `predictsegmentation.pt` (~303 MB) | DeepLabV3-ResNet50 (segmentation) | `PredictSegmentation.py` | [EchoNet-Dynamic](https://echonet.github.io/dynamic/) |

```{admonition} Rename to match, and don't mix them up
:class: warning
Save the EF checkpoint as `predictEF.pt` and the segmentation checkpoint as
`predictsegmentation.pt` — the scripts load them by those names. Loading the EF
checkpoint into the segmentation script (or vice-versa) raises a `state_dict`
size-mismatch error, because the two heads have different output shapes.
```



---

## Sample data (demo)

A small **demo dataset** is bundled so you can test the scripts end-to-end before
switching to your own data.

| File | Contents |
|------|----------|
| {download}`demo.zip <../downloads/demo.zip>` | An apical-4-chamber echo video in `video/` (`0X243FDE8AE0A05B6F.avi`) and 138 single-frame `.npy` arrays in `npy/` |

Unzip it into the project root so it becomes the `demo/` folder, with two
sub-folders, one per input type:

```text
EchoNet-dynamic/
└── demo/
    ├── video/   0X243FDE8AE0A05B6F.avi   # the echo video
    └── npy/     frame_0000.npy … (138)   # single-frame arrays
```

The default `INPUT_DIR` is `demo/video/`, so both prediction scripts run on the
video straight away. To process the `.npy` frames instead, change `INPUT_DIR` to
`demo/npy/` in `PredictSegmentation.py`:

- **EF prediction** — `demo/video/` (default); `PredictEF.py` reads the `.avi`.
- **Segmentation — video** — `demo/video/` (default); processed by Module B.
- **Segmentation — `.npy` frames** — set `INPUT_DIR` to `demo/npy/`.

```{admonition} Point at a sub-folder, not at the demo root
:class: warning
Both scripts scan only the **top level** of `INPUT_DIR` — sub-folders are not
searched. Pointing `INPUT_DIR` at `demo/` itself would find **nothing**, because
the files live one level down in `video/` and `npy/`. That is also why the video
and the frames are two separate runs.
```

---

## Ground-truth labels (for validation)

To check the scripts quantitatively (see
[Results & Troubleshooting](07_results_troubleshooting)), use the two
EchoNet-Dynamic label files. `FileList.csv` is small enough to grab here;
`VolumeTracings.csv` (~30 MB) comes from the dataset:

| File | Contents |
|------|----------|
| {download}`FileList.csv <../downloads/FileList.csv>` | Per-video true `EF`, `ESV`, `EDV`, `Split` and frame size/rate |
| [`VolumeTracings.csv`](https://echonet.github.io/dynamic/) (~30 MB) | LV boundary tracings (`X1,Y1,X2,Y2,Frame`) on the ED/ES frame of each video |

```{admonition} The demo video is already labelled
:class: tip
Both files include the bundled demo video (`0X243FDE8AE0A05B6F`, true EF
**29.9 %**), so you can validate the demo end-to-end without the full dataset.
`VolumeTracings.csv` is ~30 MB.
```

---

## After downloading

1. Arrange the files into the project layout — `scripts/`, `stats/` and `demo/`
   side by side inside one `EchoNet-dynamic/` folder:

   ```text
   EchoNet-dynamic/
   ├── scripts/   PredictSegmentation.py · PredictEF.py · CalculateStats.py
   ├── stats/     predictsegmentation.pt · predictEF.pt
   └── demo/      video/ · npy/
   ```

2. Open each script and set its path block. To use your **own** data, point
   `INPUT_DIR` at it; results land in `demo_output\`.
3. Keep the default `MEAN`/`STD` for EchoNet-style data, or recompute them with
   `CalculateStats.py` (see [Normalization Statistics](04_normalization_stats)).
4. Run a script — full walkthroughs are in
   [Segmentation](05_segmentation) and [EF Prediction](06_ejection_fraction).

Need the environment first? See [Installation & Setup](02_installation).
