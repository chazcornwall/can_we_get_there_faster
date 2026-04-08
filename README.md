# can_we_get_there_faster
Code for OMPL experiments from corresponding IEEE RAL paper. Tested in Ubuntu 22.04.

TODO: Include citation

## Changes to OMPL

We add a `bias_adjustment` parameter in RRT*, Informed RRT*, BIT*, EIT*, and BLIT* corresponding to the algorithm
described in our paper. This parameter is added using the `Planner::declareParam<bool>` API in each of the planner's constructors. 

## Run experiments

1. Use `git submodule init` and `git submodule update` to use the forked OMPL library
2. Follow the build instructions in the [OMPL README.md](../chazcornwall/ompl/README.md)
3. Open [run_experiment.cpp](../chazcornwall/ompl/experiments/run_experiment.cpp)
4. Change the `rootpath` where data is stored:

```
    ex::DataManagerParams params;
    params.rootpath = "CHANGE ME";
    params.directory_names = {&ex::EnvironmentMap, &ex::PlannerMap, &ex::MaxRadiusMap, &ex::PlanningTimeMap};
    params.file_names = &ex::FileMap;
    auto dm = ex::DataManager(params);
```
5. Define the experiment by selecting the range of variables to change in the experiment:

```
    ex::DataManagerParams params;
    params.directory_names = {&ex::EnvironmentMap, &ex::PlannerMap, &ex::MaxRadiusMap, &ex::PlanningTimeMap};
    params.file_names = &ex::FileMap;
    auto dm = ex::DataManager(params);

    // Define range (inclusive) of possible environments to evaluate
    ex::Environment first_env = ex::Environment::EMPTY;
    ex::Environment last_env = ex::Environment::EMPTY;

    // Define range (inclusive) of planners to evaluate
    ex::Planner first_planner = ex::IRRT_STAR;
    ex::Planner last_planner = ex::IRRT_STAR;

    // Define range (inclusive) of planning times to evaluate
    ex::PlanningTime first_time = ex::T_0__0_0_1;
    ex::PlanningTime last_time = ex::T_0__0_0_1;

    // Define range (inclusive) of max radii to evaluate
    ex::MaxRadius first_radius = ex::R_DEFAULT;
    ex::MaxRadius last_radius = ex::R_DEFAULT;
```

5. Open [data_directory.hpp](../chazcornwall/ompl/experiments/cpputils/data_directory.hpp) to see available options for experiment variables.
    * The `Map` data types define the naming convention used when data is stored
    * The `enum` data types define the variables. Since enums correspond to integers, the `enum` definitions will show which variables will be evaluated in the experiment. 
6. Run the experiment from this repo's root directory: `./ompl/build/Release/experiments/run_experiment`
7. Visualize data: `python3 ompl/experiments/visualizy.py`