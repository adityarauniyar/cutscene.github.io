# Database vs. Query for DINOv2 ViT-G

Environment setup

```bash
data_dir="/home2/avneesh.mishra/Documents/vl-vpr/datasets_vg/datasets"
cache_dir="/scratch/avneesh.mishra/vl-vpr/cache/new_web"
nc=16
```



## Needed Videos

See [this thread on Slack](https://iiit-rrc.slack.com/archives/C04UFEUA2MC/p1689159095300859)

- [x] **First Row**: DINOv2 ViT-G L31 Value, 8 clusters: Domain specific vocabulary: `indoor`.
  - Database trajectory (_first column_) and query trajectory (_second column_)
- [x] **Second Row**: Database trajectory. Domain specific vocabulary: `indoor`.
  - DINO (v1) ViT-S L9 Key, 8 clusters (_first column_)
  - DINOv2 ViT-S L10 Value, 8 clusters (_second column_)
  - ~~DINOv2 ViT-G L31 Value, 8 clusters (_third column_)~~ (same as first column of first row)
- [x] **Third Row**: Layer ablations (on single image) for DINO and DINOv2. Domain specific vocabulary: `indoor`.
  - DINO (v1) ViT-S Key, 8 clusters (_first column_)
  - DINOv2 ViT-S Value, 8 clusters (_second column_)
  - DINOv2 ViT-G Value, 8 clusters (_third column_)



## Cluster Centers

Environment

```bash
cluster_cdir="${cache_dir}/video_clusters"
```

All cluster centers in [my OneDrive](https://iiitaphyd-my.sharepoint.com/:f:/g/personal/avneesh_mishra_research_iiit_ac_in/EgY5TrS-WWtCtlor09D1azwByn7gctCEGgHg_XHYxoH5Og) `Results > Cache > video_clusters`

### DINOv1

```bash
cache_dir="/scratch/avneesh.mishra/vl-vpr/cache/video_clusters"
dino_layers=(`echo {0..11}`)
global_vocabs=("indoor" "urban" "aerial" "hawkins" "laurel_caverns")

# 60 runs total
```

```bash
bash ./scripts/dino_global_vocab_vlad_ablations2.sh $CUDA_VISIBLE_DEVICES
```

Cluster centers are saved in `/scratch/avneesh.mishra/vl-vpr/cache/video_clusters/vpr_global_dino_v1` within folder names like `l0_key_c8` (layer 0, key facet, 8 clusters). In this, select domain (like `indoor`) folder, and then file is `c_centers.pt`.

### DINOv2

#### ViT-G Layers

```bash
cache_dir="/scratch/avneesh.mishra/vl-vpr/cache/video_clusters"
dino_models_layers=(
    "dinov2_vitg14, `echo {39..20..1}`"
)
global_vocabs=("indoor" "urban" "aerial" "hawkins" "laurel_caverns")

# 100 runs
```

```bash
bash ./scripts/dino_v2_global_vocab_vlad_ablations2.sh $CUDA_VISIBLE_DEVICES
```



```bash
cache_dir="/scratch/avneesh.mishra/vl-vpr/cache/video_clusters"
dino_models_layers=(
    "dinov2_vitg14, `echo {19..0..1}`"
)
global_vocabs=("indoor" "urban" "aerial" "hawkins" "laurel_caverns")

# 100 runs
```

```bash
bash ./scripts/dino_v2_global_vocab_vlad_ablations4.sh $CUDA_VISIBLE_DEVICES
```

Cluster centers are saved in `/scratch/avneesh.mishra/vl-vpr/cache/video_clusters/vpr_global_vocab` within 

#### ViT-S Layers

```bash
cache_dir="/scratch/avneesh.mishra/vl-vpr/cache/video_clusters"
dino_models_layers=(
    "dinov2_vits14, `echo {11..0..1}`"
)
global_vocabs=("indoor" "urban" "aerial" "hawkins" "laurel_caverns")

# 60 runs
```

```bash
bash ./scripts/dino_v2_global_vocab_vlad_ablations3.sh $CUDA_VISIBLE_DEVICES
```





## Visualizations

```bash
cluster_cdir="${cache_dir}/video_clusters"
nc=8
```



### Gardens

Indoor vocabulary

#### DINOv2 ViT-G

```bash
dino_model="dinov2_vitg14"
```

Visualize all images in database and query set as GIFs (with cluster visualisation)

Make sure that the images are being saved in GIFs (each layer will have its own GIF spanning all images) at 5 FPS.

Single image, all layers: `row3_col3` (make sure you're saving `all layers across each image` GIF)

```bash
for dl in {0..39}; do
	python ./scripts/dino_v2_vlad_viz_layers.py --exp-id webs7 --model-type dinov2_vitg14 --num-clusters $nc --desc-layers ${dl} --desc-facet value --qu-indices 34 --qu-in-db --override-vlad-cdir "${cluster_cdir}/vpr_global_vocab/${dino_model}/l${dl}_value_c${nc}/indoor" --prog.cache-dir $cache_dir --prog.data-vg-dir $data_dir --cache-vlad-descs --prog.vg-dataset-name gardens
done
python ./scripts/test_merge_gifs.py
```

Database images: `row1_col1`

```bash
python ./scripts/dino_v2_vlad_viz_layers.py --exp-id webs1 --model-type dinov2_vitg14 --num-clusters $nc --desc-layers 31 --desc-facet value --qu-indices {0..199} --qu-in-db --override-vlad-cdir "${cluster_cdir}/vpr_global_vocab/${dino_model}/l31_value_c${nc}/indoor" --prog.cache-dir $cache_dir --prog.data-vg-dir $data_dir --cache-vlad-descs --prog.vg-dataset-name gardens --save-gif
```

Query images: `row1_col2`

```bash
python ./scripts/dino_v2_vlad_viz_layers.py --exp-id webs2 --model-type dinov2_vitg14 --num-clusters $nc --desc-layers 31 --desc-facet value --qu-indices {0..199} --override-vlad-cdir "${cluster_cdir}/vpr_global_vocab/${dino_model}/l31_value_c${nc}/indoor" --prog.cache-dir $cache_dir --prog.data-vg-dir $data_dir --cache-vlad-descs --prog.vg-dataset-name gardens --save-gif
```

#### DINOv2 ViT-S

Single image, all layers: `row3_col2` (make sure you're saving `all layers across each image` GIF)

```bash
for dl in {0..11}; do
	python ./scripts/dino_v2_vlad_viz_layers.py --exp-id webs6 --model-type dinov2_vits14 --num-clusters $nc --desc-layers ${dl} --desc-facet value --qu-indices 34 --qu-in-db --override-vlad-cdir "${cluster_cdir}/vpr_global_vocab/dinov2_vits14/l${dl}_value_c${nc}/indoor" --prog.cache-dir $cache_dir --prog.data-vg-dir $data_dir --cache-vlad-descs --prog.vg-dataset-name gardens
done
python ./scripts/test_merge_gifs.py
```

Database trajectory: `row2_col2`

```bash
python ./scripts/dino_v2_vlad_viz_layers.py --exp-id webs4 --model-type dinov2_vits14 --num-clusters $nc --desc-layers 10 --desc-facet value --qu-indices {0..199} --qu-in-db --override-vlad-cdir "${cluster_cdir}/vpr_global_vocab/dinov2_vits14/l10_value_c${nc}/indoor" --prog.cache-dir $cache_dir --prog.data-vg-dir $data_dir --cache-vlad-descs --prog.vg-dataset-name gardens --save-gif
```



#### DINOv1 ViT-S

Single image, all layers: `row3_col1` (make sure you're saving `all layers across each image` GIF)

```bash
for dl in {0..11}; do
	python ./scripts/dino_vlad_viz_layers.py --exp-id webs5 --model-type dino_vits8 --num-clusters $nc --desc-layers ${dl} --desc-facet key --qu-indices 34 --qu-in-db --override-vlad-cdir "${cluster_cdir}/vpr_global_dino_v1/l${dl}_key_c${nc}/indoor" --prog.cache-dir $cache_dir --prog.data-vg-dir $data_dir --cache-vlad-descs --prog.vg-dataset-name gardens
done
python ./scripts/test_merge_gifs.py
```

Database trajectory: `row2_col2`

```bash
python ./scripts/dino_vlad_viz_layers.py --exp-id webs3 --model-type dino_vits8 --num-clusters $nc --desc-layers 9 --desc-facet key --qu-indices {0..199} --qu-in-db --override-vlad-cdir "${cluster_cdir}/vpr_global_dino_v1/l9_key_c${nc}/indoor" --prog.cache-dir $cache_dir --prog.data-vg-dir $data_dir --cache-vlad-descs --prog.vg-dataset-name gardens --save-gif
```



