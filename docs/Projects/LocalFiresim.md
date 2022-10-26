# Drydock Local-FPGA Co-Simulation Setup Doc

## Before you start

Firesim needs a Vitis U250 FPGA to work. Luckily there are some at the firesim1 millennium machine at berkely. You can ssh into it using: 
`ssh my_username@firesim1.millennium.berkeley.edu`

However, the FPGAs at this machine is shared resources. There are 2 FPGAs plugged there. There might be other people running other stuffs on other FPGAs, so there needs to be an assignment of FPGAs. Please talk to **Abe** in SLICE lab; the FPGA we are assigned should be in slot 1, and slot 2 is used by other users. (slot0 and slot3 should be unplugged now).

## Setting Up Firesim

This section is adapted from the [Firesim Documentation](https://docs.fires.im/en/stable/Advanced-Usage/Vitis.html#setup). Shoutout to the firesim team for the great releases and Abe for helping me setting it up.

First thing first, make a new directory in scratch.

    cd /scratch
    mkdir [username]
    cd [username]

make sure you have installed Vitis/Vivado and XRT. They should be installed already on the millennium machine. However, you need to source them properly to access these software. Copy this example bashrc (written by Abe originally) to source everything you need. **Make sure you substitue all [username] with your actual username!**

    # example .bashrc

    # Quick way to get home
    export SCRATCH_HOME=/scratch/[username]

    ####################################
    # SERVER Specific Configuration
    ####################################

    export LD_LIBRARY_PATH=$SCRATCH_HOME/local-lib${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

    # Quickly call the vlsi script for the tools
    source /ecad/tools/vlsi.bashrc
    export VCS_LIC_EXPIRE_WARNING=0

    # Set SBT to cache locally
    # Used in Chipyard SBT invocation and $SCRATCH_HOME/bin
    export CACHE_OPTS="-Dsbt.ivy.home=$SCRATCH_HOME/.ivy2 -Dsbt.global.base=$SCRATCH_HOME/.sbt -Dsbt.boot.directory=$SCRATCH_HOME/.sbt/boot/"
    export JVM_OPTS="-Xmx8G -Xss8M -Djava.io.tmpdir=$SCRATCH_HOME/.java_tmp"

    # support newer chipyard
    export JAVA_OPTS="$JVM_OPTS"
    export SBT_OPTS="$CACHE_OPTS"

    export COURSIER_CACHE=$SCRATCH_HOME/.coursier-cache

    # only source on firesim1
    if [ $(uname -n) == "firesim1" ]; then
        # Get Vitis (and Vivado) 2020.2
        source /ecad/tools/xilinx/Vitis/2021.1/settings64.sh > /dev/null

        # Get XRT
        source /opt/xilinx/xrt/setup.sh > /dev/null
    fi

Great! Now let's clone firesim and set it up. 

Run the following commands:

    git clone https://github.com/firesim/firesim

    cd firesim

    git checkout 1.15.1

This next step should require sudo access when initializing conda, so might fail? Please run it, and check if conda is initialized.

    ./scripts/machine-launch-script.sh

    conda --version
    >>> conda 4.12.0

*(If conda is not initialized properly, please reach out to me. I will double check.)*

Anyways, if things goes as expected, the next step is to setup the rest of the firesim repo. 

    // unofficial step
    pip install conda-lock

    ./build-setup.sh
    >>> ... Setup complete! ...

    source sourceme-f1-manager.sh

    firesim managerinit --platform vitis

Great! At this point, your firesim repo is already setup, and you're ready to go!

## Test Building a Simulation

Let's test if firesim works as expected, by building a very small rocket core and verify it simulates correctly. Now, before you start, notice that we need to source a bunch of scripts. For our purpose, it should be:

        source env.sh
        source sourceme-f1-manager.sh

You may want to add this into your `~/.bashrc` to avoid doing it everytime. 

Now let's **add** the following build recipe to your `deploy/config_build_recipes.yaml` file. This is where you will specify the bitstream you want to generate from. 

    firesim_rocket_singlecore_no_nic:
        DESIGN: FireSim
        TARGET_CONFIG: FireSimRocketConfig
        PLATFORM_CONFIG: BaseVitisConfig
        deploy_triplet: null
        post_build_hook: null
        metasim_customruntimeconfig: null
        bit_builder_recipe: bit-builder-recipes/vitis.yaml

**Importantly**, notice that you need to configure the platform and bit builder recipe to **vitis** instead of **f1**. 

Now, let's create a building directory in your scratch directory, where firesim will dump the bitstreams it has built there.

    cd /scratch/[username]
    mkdir firesim_build_temp

We can now modify config_build.yaml, which is the configuration file firesim will refer to when running `firesim buildbitstream`. Leaving other parts intact, we should add: 

    ...
    build_farm:
        // make sure it says "externally provisioned"
        base_recipe: build-farm-recipes/externally_provisioned.yaml
        recipe_arg_overrides:
            // This is the line you should touch!
            default_build_dir: /scratch/[username]/firesim_build_temp
    ...

    builds_to_run:
        //delete all other stuffs in this section and add
        - firesim_rocket_singlecore_no_nic

Now, we're ready to build a bitstream. Since it will probably take hours, let's create a screen session (or tmux) for it.

    screen -S build

    source env.sh
    source sourceme-f1-manager.sh

    firesim buildbitstream

Take a nap or drink some tea... See you in a few hours.