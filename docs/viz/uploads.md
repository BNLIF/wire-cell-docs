[TOC]

# JSON Schema

BEE can be used to display results from any algorithms that produce *3D space points*.
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
    "geom"     : "uboone",
    "type"     : "wire-cell"
}
```

In the JSON file, only the `x`, `y`, and `z` variables are required, all others are optional.
Their descriptions are summarized below:

| Name | Description | Default |
| ---- | ----------- | ------- |
| x | array of x coordinates of the 3D space points [cm] | (required) |
| y | array of y coordinates of the 3D space points [cm] | (required) |
| z | array of z coordinates of the 3D space points [cm] | (required) |
| q | array of charges for the 3D space points [number of electrons] | [0,..] |
| runNo | DAQ run number | 0 |
| subRunNo | DAQ subrun number | 0 |
| eventNo | DAQ event number | 0 |
| geom | name of the detector geometry ("uboone", "dune35t") | "uboone" |
| type | name of the algorithm | "" |

# Upload File Structure

BEE supports user uploads through drag-and-drop to the dropzone on [the BEE homepage](http://www.phy.bnl.gov/wire-cell/bee/).
The uploaded file must be a `.zip` file that contains the following structure

```
myfile.zip
└── data
    ├── 0
    │   ├── 0-myAlg1.json
    │   ├── 0-myAlg2.json
    │   ├── 0-myAlg3.json
    ├── 1
    │   ├── 1-myAlg1.json
    │   ├── 1-myAlg1.json
    │   ├── 1-myAlg1.json

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