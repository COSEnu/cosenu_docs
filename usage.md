<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Usage

Along with the main simulation engine `COSE`$$\nu$$, we provide simple interface written in
`python` to reduce the difficulties of setting up and running the simulation. The enitre
process of the simulation can be thought of as consisting of following steps.

- Setting up.
- Initializing the grid points.
- Running the simulation.
- Analysing the results.

In the following we will explain the details of each step.
# Setting up a simulation
## Setting up the spatial grid

Open `COSEnu/configs.yaml` file. The domain spacific part typically
looks like what's shown below.

```yml
# Spatial domain specifics.
zrange: [-600, 600]
nzs: [1000]
CFLS: [0.4]
```

`zrange`: Start and end values for the domain required for the simulation. Both satrt and end values are necessary.

`nzs` : Values of resolutions for which we want to run the simulations. In principle we can put any number of value
inside the square bracket separated by comma. However the bracket should contain at least one value.

`CFLS` : Values of the `CFL` parameter for which we want to run the simulation. Any nmber of values with
in the square brackets are allowed. The bracket should not be empty. For numerical stability each of these values
should be chosen such that $$ 0< \text{CFL} \leq 1$$.

## Setting up the velocity bins

In the current implementation we only consder the z-componant of velocity $$\text{cos}(\theta)$$
which can take values from -1 to 1. To evolve each velocity mode numerically, we treat the beam of
neutrinos whose velocity lies between $$\mathrm{v}$$ and $$\mathrm{v} + d\mathrm{v}$$ as a single beam
with velocity $$\mathrm{v} + d\mathrm{v}/2$$. The value of $$d\mathrm{v}$$can be estimated from the number of
velocity bins $$\text{N}_{\mathrm{v}_z}$$. The starting and ending values of $$\mathrm{v}$$ and, the
resolution of the phsase-space can be set as below.

```yml
# Phase-space specifics.
v0: -1.0
v1: 1.0
nvzs: [50]
```

`v0` : Starting value of the velocity range.

`v1` : Ending value of the velocity range.

`nvzs` : Number of angular bins we require for the simulation.

## Specifying the output folders

The name of the folders to store the outputs from the simulation can be specified as follows.

```yml
# Folder names
folder_fv: "outputs-from-fv"
folder_fd: "outputs-from-fd"
```

`folder_fv` : Create folder with specifid name to store the results from simulation with finite
            volume (FV) scheme.
              
`folder_fd` : Create folder with specifid name to store the results from simulation with finite
              difference (FD) scheme.

Inside each of these folders, sub-folders will be created to store the output files from each configuration. 
Each of these sub-folders will be named with their `ID`. By default, the `ID` takes the form `Nz_Nvz_CFL`.

## Number of iterations

The number of iterations for which the simulation should be carried out can be specified as follows.
```yml
# Number of iterations = int(end_time/dt)
# dt = CFL*dz
end_time: 100
```

`end_time` : End time for the simulation in physical units. Given the physical end time the number of iterations 
are estimated using $$ \rm{N\_ITER} = \rm{end\_time}/dt$$, where $$ dt = \text{CFL}\times dz$$.

## Switching the advection

One can also switch on and off the advection throught the configuration file.


```yml
# Switch on/off advection.
advection_off: False # Options -> True, False
```

`advection_off`: Accepts Boolean values. Setting `advection_off` to `False` ensure the advection is switched on while setting it to 
`True` will solve for the ordinary differential equation (ODE) in time (Eq. (1) without advection).

## Specifying the oscillation parameters

To set up both vacuum and collective neutino oscillation related parameters, we can use the configuration file as follows.

```yml
# Switch on/off vacuum oscillation
vac_osc_on: False # options -> True, False
pmo: 1.0 # 1.0 -> normal mass ordering, -1.0 -> inverted mass ordering, 0.0 -> no vacuum term.
omega: 1.0e-4 # \Dedlta m^2/2E.
theta: 37 # Vacuum mixing angle in degrees.

# Switch on/off collective oscillations
collective_osc_on: True # Options -> True, False
mu: 1.0 # \sqrt{2} G_F n_{\nu_e}

```

`vac_osc_on`: Accepts Boolean values. `True` for switching on the vacuum oscillation and `False` for switching off.
By default vacuum oscillation is turned off.

`pmo`: To specify the neutrino mass ordering. Set it to 1 for normal mass ordering and -1 for inverted mass ordering.

`omega` : Vacuum oscillation frequency.

`theta` : Vacuum mixing angle.

`collective_osc_on` : Also accepts Boolean values. `True` for turning neutino collective oscillation on and `False` for turning it off.
By default, collective oscillation is turned on.

`mu` : Indicates the streangth of $$\nu-\nu$$ interaction ($$\mu=\sqrt{2}G_Fn_{\nu_e}$$). By default, $$\mu$$ is set to 1.


## Analysis

Along with the simulation, a few subroutines to check the deviation of conserved quantities and take snapshots are included. Following 
are the details default output files and their output layout.


- **`conserved_quantities.dat` :** Stores the details of the deviations of the conserved quantities.
    - _Output layout_ : 
    $$
    \text{Time} ~~|~~ \delta P_\text{max} ~~|~~ \langle\delta P\rangle ~~|~~ \langle\delta\bar{P}\rangle ~~|~~ |M_0| 
    $$        
- **`survival_probability.dat` :** Stores the survival probabilities of $$\nu$$ and $$\bar\nu$$
    - _Output layout_ : 
    $$
    \text{Time} ~~|~~ P_{\nu_e->\nu_e} ~~|~~ P_{\bar\nu_e->\bar\nu_e}
    $$
- **`rho_t.dat` :** Stores full snap shot data of the field variables.
    - _Output layout_ :

    $$
     z ~~|~~ \mathrm{v} ~~|~~ \rho_{ee} ~~|~~ \rho_{xx} ~~|~~ \text{Re}[\rho_\text{ex}] ~~|~~ \text{Im}[\rho_\text{ex}] ~~|~~ \bar\rho_{ee} ~~|~~ \bar\rho_{xx} ~~|~~ \text{Re}[\bar\rho_\text{ex}] ~~|~~ \text{Im}[\bar\rho_\text{ex}]
    $$
    
```yml
# ANALYSIS
#----------------------------------------------------------------#

# To analyze the behaviour of the conserved quantities.
n_analyze: 100 # Total number of analysis to be carried out per job.

# To capture the evolution of the field variables for all the velocities over entire domain.
n_dump_rho: 100
```
Apart from these output files, `COSE`$$\nu$$ also store the initial state and the states of the simulation at regular intervals to binary files so that the simulation can be restarted from the last stored stat to take care of any unexpected stoppage. This is accomplished with the help of `NuOsc::write_state()/read_state()` inside the `COSEnu/src/snaps.hpp` file.


# Initializing the density matrix
The density matrix elements (aka field variables) can be initialized using the `NuOsc::initialize()` subroutine in the `COSEnu/src/initialize.hpp` file.
The code snippet that initializes the field varibles look as follows.
```c++
double signu  = 0.6;
double sigbnu = 0.53;
double alpha  = 0.9;

std::ofstream g_file(ID + "_G0.bin",std::ofstream::out | std::ofstream::binary);
if(!g_file)
{
    std::cout << "Unable to open " << ID+"_G0.bin" << " file from NuOsc::initialise." 
    << "Will not be storing initial angular profiles.\n";
}

for (int i = 0; i < nvz; i++)
{
    for (int j = 0; j < nz; j++)
    {
        G0->G[idx(i, j)] = g(vz[i], 1.0, signu);
        G0->bG[idx(i, j)] = alpha * g(vz[i], 1.0, sigbnu);
        
        v_stat->ee[idx(i, j)]    = 0.5 * G0->G[idx(i, j)] * (1.0 + eps_(Z[j], 0.0)); 
        v_stat->xx[idx(i, j)]    = 0.5 * G0->G[idx(i, j)] * (1.0 - eps_(Z[j], 0.0));
        v_stat->ex_re[idx(i, j)] = 0.5 * G0->G[idx(i, j)] * (0.0 + eps(Z[j], 0.0));
        v_stat->ex_im[idx(i, j)] = -0.0;

        v_stat->bee[idx(i, j)]    = 0.5 * G0->bG[idx(i, j)] * (1.0 + eps_(Z[j], 0.0)); 
        v_stat->bxx[idx(i, j)]    = 0.5 * G0->bG[idx(i, j)] * (1.0 - eps_(Z[j], 0.0));
        v_stat->bex_re[idx(i, j)] = 0.5 * G0->bG[idx(i, j)] * (0.0 + eps(Z[j], 0.0));
        v_stat->bex_im[idx(i, j)] = 0.0;

        g_file.write((char *)&G0->G [idx(i, j)], sizeof(double)); 
        g_file.write((char *)&G0->bG[idx(i, j)], sizeof(double));
    }
}
```

By default, the simulation is initialzed (angular and spatial distributions, and perturbations) according to the convension in [this](https://doi.org/10.1103/PhysRevD.104.103003) and [this](https://arxiv.org/abs/2203.12866) articles.

`g(...)` : Returns Gaussian angular destribution. Defined in `COSEnu/src/miscellaneous_funcs.hpp`.

`idx(i, j)` : Return the memory index for the (i, j) grid point.

`G0->G[]/G0->bG[]` : Array to store the initial angular distribution of neutrino/anti-neutrino.

`v_stat->ee[], v_stat->xx[], v_stat->ex_re[], v_stat->ex_im[]` : Arrays to store the initial values of $$\rho_{ee},~\rho_{xx},~\mathrm{Re}[\rho_{ex}]$$ and $$\mathrm{Im}[\rho_{ex}]$$ respectively.

`v_stat->bee[], v_stat->bxx[], v_stat->bex_re[], v_stat->bex_im[]` : Arrays to store the initial values of $$\bar\rho_{ee},~\bar\rho_{xx},~\mathrm{Re}[\bar\rho_{ex}]$$ and $$\mathrm{Im}[\bar\rho_{ex}]$$ respectively.

Finally the initial angular profile is stored to a binary file named `G0.bin`.


[<previous](comp_setup.md)  &#124;  [home](index.md)  &#124;  [next>](run.md) 
