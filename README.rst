
Benchmark Agreement Testing (BAT) Package
=========================================

Overview
--------
The BAT package is designed to facilitate benchmark agreement testing for NLP models. It allows users to easily compare multiple models against various benchmarks and generate comprehensive reports on their agreement.

It also supports `BenchBench <https://huggingface.co/spaces/ibm/benchbench>`_ the benchmark to compare benchmarks.

Installation
------------
To install the BAT package, you can use pip::

    pip install bat-package

Usage Example
-------------
Below is a step-by-step example of how to use the BAT package to perform agreement testing.

**Step 1: Configuration**

First, set up the configuration for the tests::

    import pandas as pd
    from bat import Tester, Config, Benchmark, Reporter
    from bat.utils import get_holistic_benchmark

    cfg = Config(
        exp_to_run="example",
        n_models_taken_list=[0],
        model_select_strategy_list=["random"],
        n_exps=10
    )

**Step 2: Fetch Model Names**

Fetch the names of the reference models to be used for scoring::

    tester = Tester(cfg=cfg)
    models_for_benchmark_scoring = tester.fetch_reference_models_names(
        reference_benchmark=get_holistic_benchmark(), n_models=20
    )
    print(models_for_benchmark_scoring)

**Step 3: Load and Prepare Benchmark**

Load a new benchmark and add an aggregate column::

    newbench_name = "fakebench"
    newbench = Benchmark(
        pd.read_csv(f"src/bat/assets/{newbench_name}.csv"),
        data_source=newbench_name,
    )
    newbench.add_aggregate(new_col_name=f"{newbench_name}_mwr")

**Step 4: Agreement Testing**

Perform all-vs-all agreement testing on the new benchmark::

    newbench_agreements = tester.all_vs_all_agreement_testing(newbench)
    reporter = Reporter()
    reporter.draw_agreements(newbench_agreements)

**Step 5: Extend and Clean Benchmark**

Extend the new benchmark with holistic data and clear repeated scenarios::

    allbench = newbench.extend(get_holistic_benchmark())
    allbench.clear_repeated_scenarios(source_to_keep=newbench_name)

**Step 6: Comprehensive Agreement Testing**

Perform comprehensive agreement testing and visualize::

    all_agreements = tester.all_vs_all_agreement_testing(allbench)
    reporter.draw_agreements(all_agreements)

Contributing
------------
Contributions to the BAT package are welcome! Please submit your pull requests or issues through our GitHub repository.

License
-------
This package is released under the MIT License.
