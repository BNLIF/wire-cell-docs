[TOC]

# JSON Schema

Bee can be used to display results from any algorithms that produce *3D space points*.
At the core, it parses a JSON file that looks like:

```json
{
    "runNo"    : "10000007",
    "subRunNo" : "16",
    "eventNo"  : "1501",
    "x"        : [1.0, 1.0, 3.0],
    "y"        : [2.0, 3.0, 4.0],
    "z"        : [3.0, 7.0, 4.5],
    "q"        : [3000, 4200, 5600],
    "nq"       : [1, 1, 1],
    "cluster_id" : [1, 1, 1],
    "real_cluster_id" : [1, 1, 1],
    "geom"     : "uboone",
    "type"     : "wire-cell",
}
```

In the JSON file, only the `x`, `y`, and `z` variables are required, all others are optional.
Their descriptions are summarized below:

| Name | Description | Default | Note |
| ---- | ----------- | ------- | ---- |
| x | array of x coordinates of the 3D points [cm] |  |required|
| y | array of y coordinates of the 3D points [cm] |  |required|
| z | array of z coordinates of the 3D points [cm] |  |required|
| q | array of charges of the 3D points [# electrons] | [0, ...] ||
| nq | array of number of points in each sampled Blob | [1, ...] |unused|
| cluster_id | array of cluster id of the 3D points | [1, ...] ||
| real_cluster_id | | [-1, ...] |only for backward compatibility|
| runNo | DAQ run number | 0 ||
| subRunNo | DAQ subrun number | 0 ||
| eventNo | DAQ event number | 0 ||
| geom | name of the detector geometry | "uboone" ||
| type | name of the algorithm | "" |unused|

The `cluster_id` is usually assigned such that the larger cluster has a smaller id.

Since all json files are transferred and parsed as text files, one should be careful to not include too many unnecessary digits in each parameter to save the bandwidth and memory usage.

# Additional files

In addition to the files that contain the reconstructed 3D points, some additional files are supported in Bee to provide extra information about the event:

- `mc.json` provides the Geant4 truth level information about the particle flow information. This file can be produced inside LArSOFT such as the [CellTree dumper](https://cdcvs.fnal.gov/redmine/projects/larreco/repository/revisions/develop/entry/larreco/WireCell/CellTree_module.cc). Example: [mc.json](https://www.phy.bnl.gov/twister/bee/set/uboone/lee/2021/wire-cell-gallery/event/0/mc/).
- `op.json` provides the reconstructed optical flash information. Example: [op.json](https://www.phy.bnl.gov/twister/bee/set/uboone/lee/2021/wire-cell-gallery/event/0/op/).
- `channel-deadarea.json` provides the dead areas viewed in Y-Z plane. Each area is represented by a polygon using its vertex coordinates `[[y1, z1], [y2, z2], [y3, z3], ...]` in cm. One should be careful not to sample additional points along a line. The array of the polyons comprises the whole dead areas.  Example: [channel-deadarea.json](https://www.phy.bnl.gov/twister/bee/set/uboone/lee/2021/wire-cell-gallery/event/0/channel-deadarea/)

# Upload File Structure

Bee supports user uploads through drag-and-drop to the dropzone on [the Bee homepage](http://www.phy.bnl.gov/wire-cell/bee/).
The uploaded file must be a `.zip` file that contains the following structure

```
myfile.zip
└── data
    ├── 0
    │   ├── 0-myAlg1.json
    │   ├── 0-myAlg2.json
    │   ├── 0-myAlg3.json
    |   ├── 0-mc.json
    |   ├── 0-op.json
    |   ├── 0-channel-deadarea.json
    ├── 1
    │   ├── 1-myAlg1.json
    │   ├── 1-myAlg1.json
    │   ├── 1-myAlg1.json
    |   ├── 1-mc.json
    |   ├── 1-op.json
    |   ├── 1-channel-deadarea.json

```

or, more specifically:

- the top directory must be named `data/`
- each event has its own numbered sub-directory
- each algorithm has its own JSON file
- the JSON file should be named as `{eventID}-{algName}.json`
- the event ID should be sequential and start from 0

Once the directory and files are ready, the `.zip` file can be created with any prefered zip compression tools. As an example, in linux, one can create the `myfile.zip` file for upload by running

```bash
zip -r myfile data
```

# Tools

## Convert Wire-Cell Output to BEE

Download the the two files from [here](https://github.com/WireCell/wire-cell-bee/tree/master/tools/wire-cell-to-bee), then run

```bash
python dump_json.py [filename] [alg1 alg2 ...]
```

The currently available Wire-Cell algorithms are `simple`, `charge`, `true`, and `deblob`.
At the end of the run, a `to_upload.zip` is created and can be drag-to-upload to [BEE](https://www.phy.bnl.gov/twister/bee)

Some notes:

- The default algorithms to run are `simple` and `charge` if not specified
- The script expects the ROOT file ending with `_{eventID}.root`,
and will actually run through all files in the same directory that have the same prefix.
- If your files are in the same directory but have different prefix,
you can use a special syntax `python dump_json.py 'yourdir/*_*.root' [alg1 alg2 ...]`. Please don't foget the single quotes.


## Convert LArSoft Output to BEE

Assuming that you have `larsoft` installed,
(if not, follow [here](https://cdcvs.fnal.gov/redmine/projects/dunetpc/wiki/_Tutorial_)),
simply run the following script to convert your larsoft output to BEE

```bash
lar -c celltree_dune35t.fcl [filename]
```

or, for MicroBooNE / protoDUNE, change the corresponding `.fcl` file to
`celltree_uboone.fcl` / `celltree_protodune.fcl`.


It will create a `bee/bee_upload.zip` file under your working directory.
You can then drag-and-drop the zip file to [Bee](https://www.phy.bnl.gov/twister/bee) to view the results
(and MC tracks).

The `CellTree` module is located under [`larreco/WireCell/`](https://cdcvs.fnal.gov/redmine/projects/larreco/repository/revisions/develop/entry/larreco/WireCell/CellTree_module.cc)
It works for any larsoft output that contains *Space Points*.
To change or add different `SpacePoint` algorithms (it supports multiple algorithms),
copy the `celltree_{detector}.fcl` file to your working directory
and modify the line `SpacePointLabels: ["pmtrackdc"]` to include your favorate algorithms.
