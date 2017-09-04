# RAPPOR – Analysis

This is a Python reimplementation of the analysis part of RAPPOR ([paper](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/42852.pdf) | [original repository](https://github.com/google/rappor) | [updated repository](https://github.com/Alexrs95/rappor)).
Concretely, there are two different notebooks available:
- [RAPPOR-Production](blob/master/RAPPOR-Production.ipynb): Can be used to perform an entire RAPPOR analysis using all the components that we deemed useful. Additionally, test datasets can be dynamically generated using Spark.
- [RAPPOR-Prototyping](blob/master/RAPPOR-Prototyping.ipynb): Contains a lot of explanations for the individual steps in RAPPOR and a lot of experimenting with different ideas. This notebook works with datasets generated from the [original](https://github.com/Alexrs95/rappor) repository.

Both notebooks need to be run from within this repository as they require the files located in the `clients/` folder.

## Installing dependencies

Generally, only the usual SciPy tech stack is required.

```sh
$ pip install scipy numpy matplotlib pandas sklearn
```

To be able to use the same hash functions as the client part, some files were already copied from the other repository and put into the `client` folder.

---

## Prototyping notebook

### Generating data

Before we can start running the analysis part, we need to have sopme datasets to work on.

To generate these, clone the repository with Alejandro's updates: 

```sh
$ git clone https://github.com/Alexrs95/rappor
```

Then, run one of the following commands, depending on which distribution you want to use: 

```sh
$ ./regtest.sh run-seq 'r-zipf1-small-sim_final'
$ ./regtest.sh run-seq 'r-zipf1.5-small-sim_final'
$ ./regtest.sh run-seq 'r-gauss-small-sim_final'
$ ./regtest.sh run-seq 'r-exp-small-sim_final'
$ ./regtest.sh run-seq 'r-unif-small-sim_final'
```

Alternatively, all these datasets can be generated with one command:

```sh
$ ./regtest.sh run-seq 'r-zipf1-small-sim_final|r-zipf1.5-small-sim_final|r-gauss-small-sim_final|r-exp-small-sim_final|r-unif-small-sim_final'
```

There are many other datasets, a full list can be generated by running `tests/regtest_spec.py`.

Generally, we want to use the datasets with the `final` parameters, as listed [here](https://bugzilla.mozilla.org/show_bug.cgi?id=1379195).
Using `small` means we work on reports from 1,000,000 clients with 100 unique values. This is a reasonable dataset size to still run the analysis on a laptop.
For testing, looking at all distributions is useful. The real distribution for our use case is probably most similar to `zipf1.5`.

Just for seeing how important using the tuned parameters is, a different dataset can be generated:
```sh
$ ./regtest.sh run-seq 'r-gauss-small-sim_bloom_filter1_1'
$ ./regtest.sh run-seq 'r-gauss-small-sim_bloom_filter1_1|r-zipf1-small-sim_final|r-zipf1.5-small-sim_final|r-gauss-small-sim_final|r-exp-small-sim_final|r-unif-small-sim_final'
```

### Running the analysis

At the top of the Jupyter notebook, the path to the generated data needs to be adapted. Afterwards, just run all cells to see the results.
Depending on the data and parameters used, this might take a while.
