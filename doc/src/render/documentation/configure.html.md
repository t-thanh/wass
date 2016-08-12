---
title: "Configuration"
layout: "default"
isPage: true
menuOrder: 4
---

# Pipeline Configuration

Before using WASS to reconstruct your stereo data, two different components must be configured:

1. WASSjs should be made aware of the location of your data, the number of frames, desired output directory, etc
2. The pipeline executables (especially ```wass_stereo```) should be tuned so that the best possible performance
can be achieved.

This page will help you configure WASSjs. Refer to [Matcher Configuration](/wass/documentation/matcher.html) or
[Stereo Configuration](/wass/documentation/stereo.html) for a guide configuring ```wass_match``` and ```wass_stereo```
respectively.

## WASSjs general configuration

General settings regarding the whole WASS pipeline can be set by editing the file ```<WASS_ROOT>/WASSjs/settings.json```. This
file is loaded once WASSjs is started and should be independent from the specific dataset to be reconstructed.

Here is an example of the default ```settings.json```:

```
{
    "pipeline_dir": "../dist/bin/",
    "prepare_exe": "wass_prepare",
    "matcher_exe": "wass_match",
    "autocalibrate_exe": "wass_autocalibrate",
    "stereo_exe": "wass_stereo",
    "http_port": 3000,
    "num_frames_to_match": 50,
    "prepare_parallel_jobs": 4,
    "match_parallel_jobs": 4,
    "stereo_parallel_jobs": 4
}
```

When editing this file it is important to maintain its structure so that it
respects the [JSON data-interchange format](http://www.json.org).  Just modify
the required values without altering the structure (ie. keep the brakets and so
on) so that it can be properly read by WASSjs.

Here is a short explanation of all the relevant options:

|   key    |     value          |
|---------|--------------------|
| ```pipeline_dir```           |  directory (either absolute or relative) of the pipeline executables  |
| ```prepare_exe```           |  name of the prepare executable (no need to change this)  |
| ```matcher_exe```           |  name of the matcher executable (no need to change this)  |
| ```autocalibrate_exe```           |  name of the autocalibrate executable (no need to change this)  |
| ```stereo_exe```           |  name of the stereo executable (no need to change this)  |
| ```http_port```           |  port used by the internal http server  |
| ```num_frames_to_match```           |  The maximum number of stereo frames to be matched in a sequence. Small number of frames will give a faster but less accurate auto-calibration. Vice-versa |
| ```prepare_parallel_jobs```           | Number of prepare jobs to be run in parallel (set this number to the number of cpu cores of your machine | 
| ```match_parallel_jobs```           | Number of match jobs to be run in parallel (set this number to the number of cpu cores of your machine | 
| ```stereo_parallel_jobs```           | Number of stereo reconstruction jobs to be run in parallel (set this number to the number of cpu cores of your machine | 




## Data-specific configuration

All the setting relative to a specific image set to be reconstructed are contained in the file ```<WASS_ROOT>/WASSjs/worksession.json```.
Once a new stereo sequence is acquired, start by placing all the stereo images in a directory of your choice following the naming convention explained
in the [Getting-started](/wass/documentation/getting_started.html) section (or follow the example set given with WASS_TEST.zip).

Then, edit the file ```<WASS_ROOT>/WASSjs/worksession.json``` to set the correct input/output directories together with the reconstruction
settings. 

The default ```worksession.json``` to be used with the provided WASS_TEST dataset should look like the following:
 

```
{
    "cam0_datadir":"../test/WASS_TEST/W07/input/cam0/",
    "cam1_datadir":"../test/WASS_TEST/W07/input/cam1/",
    "workdir":"../test/output_W07/",
    "confdir":"../test/WASS_TEST/W07/config/",
    "savediskspace":false,
    "keepimages":true,
    "zipoutput":true,
    "match_config_file":"matcher_config.txt",
    "dense_stereo_config_file":"stereo_config.txt",
    "wdir_frames":[],
    "seq_start":0,
    "seq_end":6
}
```

Here is a short explanation of all the relevant options:

|   key    |     value          |
|---------|--------------------|
| ```cam0_datadir```           |  directory (either absolute or relative) containing all the first camera images  |
| ```cam1_datadir```           |  directory (either absolute or relative) containing all the second camera images  |
| ```workdir```           |  output directory  |
| ```confdir```           |  directory (either absolute or relative) containing the calibration and pipeline executables configuration files  |
| ```savediskspace```           |  ***true***: Save some disk space by removing unnecessary files after the reconstruction. <br /> ***false***: Keep all the temporary files generated by the pipeline. Also, keep the undistorted high-resolution images  | 
| ```keepimages```           |  ***true***: Keep the undistorted high-resolution images (even if ```savediskspace``` is set to ***true***) <br /> ***false***: do nothing. | 
| ```zipoutput```           |  ***true***: Zip each working directory <br /> ***false***: keep the reconstructed output directories as-is. | 
| ```match_config_file```   | ```wass_match``` configuration file name (assumed to be located under ```confdir``` directory) | 
| ```dense_stereo_config_file```   | ```wass_stereo``` configuration file name (assumed to be located under ```confdir``` directory) | 
| ```wdir_frames```   | List of all the working directories. ***Do not edit this option as it will be automatically populated by WASSjs*** |
| ```seq_start```   | Index of the first frame of the stereo sequence |
| ```seq_end```   | Index of the last frame of the stereo sequence |
