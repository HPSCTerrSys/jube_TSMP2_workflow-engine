# jube_TSMP2_workflow-engine
This `JUBE` file installs the [TSMP2_workflow-engine](https://github.com/HPSCTerrSys/TSMP2_workflow-engine) and is able to build and run the TSMP2_wfe upon request (using tags).

### How to use it
If the working directory to the benchmark is:
```
working_dir = path_to_benchamark_folder/benchmark
```

1) Upload JUBE:
```
ml JUBE
```

2) Run JUBE:

In the `working_dir` that contains the “jube_tsmp2_wfe.yml” file, submit the following command, replacing the tags (see section Tags below):
```
jube -v run -r -e --tag system_name execution_mode download_data tsmp_combination environment --outpath ./outpath jube_tsmp2_wfe.yml
```
##### NOTE: Tags must be provided after `--tag` (in any order).

`-v` is verbose, displays the log output during the run

`-e` jube run will exit if there is an error

`-r` run result after finishing run command (this will also start analysis)

`--outpath` defines where the benchmark is going to be stored

For more details on how to run JUBE, have a look at the JUBE [documentation](https://apps.fz-juelich.de/jsc/jube/docu/commandline.html).

3) Continue an existing benchmark (after simulation is finished):
```
jube continue ./outpath
```

### Examples
1) To build and run TSMP2 with `icon` + `eclm` on `jureca` using the environment `jsc.2025.gnu.openmpi`, the jube command would be:
```
jube -v run -r -e --tag jureca full icon eclm jsc.2025.gnu.openmpi --outpath ./outpath jube_tsmp2_wfe.yml
```
Since no `download_data` is defined, it will NOT download the data (since it uses the default option).

2) To only build a fully-coupled TSMP2 (`icon + eclm + parflow`) on `juwels` using the environment `jsc.2025.intel.psmpi` and no download data, the jube command would be:
```
jube -v run -r -e --tag juwels build icon eclm parflow nodata jsc.2025.intel.psmpi --outpath ./outpath jube_tsmp2_wfe.yml
```

3) If only the system name is provided (e.g., `juwels`):
```
jube -v run -r -e --tag juwels --outpath ./outpath jube_tsmp2_wfe.yml
```   
It will use default options on `juwels`: build of eclm with environment `default.2025.env` and doesn't download data.


### Tags

`system_name`: machine where the benchmark runs.

`execution_mode`: allows to choose among `full` (build + run), `build` (only build), and `run` (only run). If no execution mode is defined, it will only build. To use `run` mode, the combination build must have been built previously. Where the combination build is `<machine + components + environment>`.

`download_data`: allows to choose between `data` (downloads data) and `nodata` (no data is downloaded). If no download data mode is defined, the data will not be downloaded. To use the `nodata` mode, data must have been downloaded previously.

`TSMP_components`: allows to choose the components for the build. Components: `icon`, `eclm`, `parflow`. If no component-combination is specified, it will build by default `eclm`.

`environment`: allows to choose among several defined environments. When no environment or `default.2025.env` are spcified, it will use `jsc.2025.gnu.openmpi`.


| Tag Name         | Tag Options                                                                 | Default Tag             | Type of Tag                             |
|------------------|-----------------------------------------------------------------------------|-------------------------|-----------------------------------------|
| system_name      | jureca, juwels, jupiter, jusuf                                              | no default option, needs to be defined | exclusive (only one can be selected)    |
| execution_mode   | full, build, run                                                             | build                   | exclusive (only one can be selected)     |
| download_data    | data, nodata                                                                 | nodata                    | exclusive (only one can be selected)     |
| TSMP_components | icon, eclm, parflow                                                          | eclm                    | multiple choices allowed                 |
| environment      | jsc.2025.intel.psmpi  <br> jsc.2025.gnu.openmpi  <br> jsc.2024.intel.psmpi <br> jsc.2023.intel.psmpi <br> default.2025.env <br> ubuntu.gnu.openmpi <br> uni-bonn.gnu.openmpi | jsc.2025.gnu.openmpi       | exclusive (only one can be selected)     |

### Directories
- Results of the benchmark will be stored at `outpath`:
```
working_dir/outpath
```

- Builds will take place here:
```
working_dir/outpath/0000##/000001_build/work/TSMP2_workflow-engine/src/TSMP2
```
Where `##` stands for the benchmark ID-number. For instance, `000000` or `000005`.

- Copy of binaries. When a build is carried out, a copy of the binaries will be stored (to be used later) here:
```
working_dir/bin
```

- Runs will take place here:
```
working_dir/outpath/0000##/000001_build/work/TSMP2_workflow-engine/run
```

-	Data will be downloaded here:
```
working_dir/download_data
```
