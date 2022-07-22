# "The Packet Number Space Debate in Multipath QUIC" Artifacts

This repository contains, as git submodules, the software required to perform the experiments done in the paper.
In addition, indications are provided to use them.


## Getting the Data and Graphs

All the measurement data, as well as a Junyper notebook to reproduce our results, are available on Zenodo: [https://doi.org/10.5281/zenodo.6323195](https://doi.org/10.5281/zenodo.6323195)

## Getting the Softwares

The fast way:

```bash
git submodule update --init
```
The `pquic` folder will contain the NS3 setup and the PQUIC implementation, while the `picoquic` folder will contain its NS3 setup and the two versions of the `picoquic` implementation.


The detailed way:

Our artifacts consist in the NS3 setup in which we run PQUIC and `picoquic`, as well as PQUIC and the modified `picoquic` implementations.
The NS3 setup is available at [https://github.com/p-quic/pquic-ns3](https://github.com/p-quic/pquic-ns3).
The branch `exp3` is used for PQUIC while the branch `exp3-picoquic-current` is used for `picoquic`.
These different versions are used for technical reasons (mainly due to the custom scripts used).
For `picoquic`, you can find the versions at [https://github.com/qdeconinck/picoquic](https://github.com/qdeconinck/picoquic), where the branch `picoquic-ns3` (commit `fa527da`) corresponds to the "original" version where the branch `picoquic-fixed-ns3` (commit `66649c1`) relates to the "fixed" version.
The PQUIC implementation is located at `https://github.com/p-quic/pquic`, relying on the `master` branch with commit `841c822`.


## Using the Softwares

First, you need to create the Docker for each NS3 version.
This can be done using the following command, tagging here the created image as `pquic-ns3-dce`.
```bash
docker build -t pquic-ns3-dce .
```
The building process takes some time and can be tricky to achieve, especially when coming up to NS3 and DCE and the underlying dependencies.
If you face difficulties to build such containers, let us know and we will see how we could distribute them through the Docker Hub.

Then, once your containers are ready, build the QUIC implementations.
For each QUIC implementation, follow their README to build them.

Finally, to run a container with a QUIC implementation, go to the repository where you created the container and run
```bash
./run.sh /path/to/the/evaluated/QUIC/implementation
```
Note that you may want to adapt the `run.sh` script to change the `picoquic-ns3-dce` occurrence to the tag you previously defined.
Also, please specify the absolute path to the QUIC implementation repository.

Once in the container, `cd /picoquic-ns3-dce/`.
You should then see the content of the `picoquic-ns3` repository.
To adapt the tests you run, you have to modify the `tests.yaml` file to the values you want to consider.
Have a look at the provided YAML file, and the custom scripts defined in the `myscripts` folder to find the different parameters you can tweak.
Finally, you can run the tests using
```bash
./testsuite.py -r /path/to/your/json/results
```
Again, prefer using absolute paths to specify your JSON file.

The provided measurement artifacts provide additional helpers to process the JSON files (that can be huge).