# cbig_network_correspondence_data

Data repository for the [`cbig_network_correspondence`](https://github.com/rubykong/cbig_network_correspondence)
toolbox. `datasets.py` in the toolbox lazy-fetches everything here from
`raw.githubusercontent.com/rubykong/cbig_network_correspondence_data/<DATA_TAG>/`
at a tagged ref -- this repo itself is never pip-installed.

New atlases are added via `scripts/release_atlas/` in the toolbox repo
(`stage_atlas.sh` / `promote_atlas.py` / `compute_overlap.py`) -- see that
directory's README for the release workflow. This file just documents what's
here.

## Layout

| Path | Contents |
|---|---|
| `atlas_config/<Data_Name>` | One INI file per atlas (`[data_info]`): category, native space, type, network-assignment flag, optional runtime threshold. Read by `grab_data_info.read_config()`. |
| `atlas_list` / `atlas_list_fullname` | Parallel, line-aligned arrays of every `Data_Name` and its human-readable full name (the table below). |
| `atlases/<space>/<Data_Category>/<Data_Name>*` | The actual atlas file(s), fanned out across every supported space: `fsaverage6`, `fs_LR_32k`, `FSLMNI2mm`, `LairdColin2mm`, `ShenColin1mm`. `Hard` atlases are a single multi-label file (`.mat` with `lh_labels`/`rh_labels` for surface spaces, `.nii.gz` for volumetric); `Soft`/`Metric` atlases are one continuous map per network (e.g. one file per ICA component). |
| `network_names/<Data_Name>` | One network name per line, same order as the atlas's labels/files. Read by `construct_network_name()` at runtime. |
| `network_assignment/<Data_Name>.mat` | `mapping` variable grouping an atlas's networks/parcels into coarser networks. Only present when `Data_NetworkAssignment` is true. |
| `overlap_results/<Data_Name>/<other_name>.mat` | Dice overlap matrix (`overlap` key) between every atlas pair, in both directions. |
| `cortical_masks/`, `templates/` | Volumetric cortical masks and reference templates per space, used during projection/warping. |
| `spin_rotations/` | Precomputed spin-test rotations per surface space. |
| `examples/` | Example exemplar map(s) for demos/tests. |

## Atlases

| `Data_Name` | `Data_FullName` | Category | Native space | Type |
|---|---|---|---|---|
| MG360J12 | Glasser2016-360+Ji2019-12 | Glasser | fs_LR_32k | Hard |
| HCPICA | HCPICA-25 | HCPICA | fs_LR_32k | Soft |
| AL20 | Laird2011-20 | Laird | LairdColin2mm | Soft |
| AS200K17 | Schaefer2018-200+Kong2019-17 | YeoLab | fsaverage6 | Hard |
| AS200Y17 | Schaefer2018-200+Yeo2011-17 | YeoLab | fsaverage6 | Hard |
| AS400K17 | Schaefer2018-400+Kong2019-17 | YeoLab | fsaverage6 | Hard |
| AS400Y17 | Schaefer2018-400+Yeo2011-17 | YeoLab | fsaverage6 | Hard |
| XS268_8 | Shen2013-268-8 | Shen | ShenColin1mm | Hard |
| XS368_8 | Shen2013-368-8 | Shen | ShenColin1mm | Hard |
| WS90_14 | Shirer2013-90-14 | Shirer | FSLMNI2mm | Hard |
| UKBICA | UKBICA-25 | UKBICA | FSLMNI2mm | Soft |
| EG286_12 | Gordon2016-286-12 | WashU | fs_LR_32k | Hard |
| TL12 | Laumann2015-12 | WashU | fs_LR_32k | Hard |
| EG17 | Gordon2017-17 | WashU | fs_LR_32k | Hard |
| TY7 | Yeo2011-7 | YeoLab | fsaverage6 | Hard |
| TY17 | Yeo2011-17 | YeoLab | fsaverage6 | Hard |
| XY200K17 | Yan2023-200+Kong2019-17 | YeoLab | fs_LR_32k | Hard |
| XY200Y17 | Yan2023-200+Yeo2011-17 | YeoLab | fs_LR_32k | Hard |
| XY400K17 | Yan2023-400+Kong2019-17 | YeoLab | fs_LR_32k | Hard |
| XY400Y17 | Yan2023-400+Yeo2011-17 | YeoLab | fs_LR_32k | Hard |
| TW_TASK_NETS | Woodward2024-Task-12 | Woodward | FSLMNI2mm | Metric |
| EG5 | Gordon2024-5 | WashU | FSLMNI2mm | Hard |
| DU15NET | Du2024-15 | Du | fsaverage6 | Hard |
| GINNA33 | Gillig2025-33 | GINNA | FSLMNI2mm | Soft |
| JacksonSC4 | Jackson2021-4 | Jackson | FSLMNI2mm | Soft |
| BL3 | Lipkin2022-3 | Lipkin | FSLMNI2mm | Soft |

`atlas_list`/`atlas_list_fullname` are the source of truth and must stay
line-aligned -- this table is generated from them plus each atlas's
`atlas_config` entry; regenerate it by hand if either file changes without
updating this README.
