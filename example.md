# An example.

Here we consider an example. We would like to compile the code for multi-core usage with the following options:

(z0, z1) = (-600, 600)
nvz = 64 and 128
nz = 5000, 10000, and 20000 
and 
CFL = 0.2 and 0.4 

We would also like to run the simulation from $$t = 0$$ to $$t = 1200$$ .

The `COSEnu/lib/config.yaml` file with vacuum oscillations turned off look like as shown below

```yaml

# CONFIGURATIONS FOR THE SIMULATION.
#----------------------------------------------------------------#

# Spatial domain specifics.
zrange :  [-600, 600]
nzs    :  [5000, 10000, 20000]
CFLS   :  [0.2, 0.4,]

#----------------------------------------------------------------#

# Phase-space specifics.
v0     : -1.0
v1     :  1.0
nvzs   :  [64, 128,]

#----------------------------------------------------------------#

boundary : periodic  # Options ->  periodic, open

#----------------------------------------------------------------#

end_time  : 1200 # Physical end time

#----------------------------------------------------------------#

# Switch on/off advection.
advection_off : False # Options -> True, False

# Switch on/off vacuum oscillation
vac_osc_on : False # options -> True, False
pmo   : 1.0        # 1.0 -> normal mass ordering, -1.0 -> inverted mass ordering, 0.0 -> no vacuum term.
omega : 1.0        # \Dedlta m^2/2E.
theta : 37         # Vacuum mixing angle in degrees.

# Switch on/off collective oscillations
collective_osc_on : True  # Options -> True, False
mu : 1.0                  # \sqrt{2} G_F n_{\nu_e}
#----------------------------------------------------------------#
# ANALYSIS
#----------------------------------------------------------------#

# To analyze the behaviour of the conserved quantities.
n_analyze : 100 # Total number of analysis to be carried out per job.

# To capture the evolution of the field variables for all the velocities over entire domain.
n_fullsnap : 3

# To capture the evolution of the field variables for all th velocity modes at given locations.
n_vsnap     : 5           # snapshot of phase-space at vsnap_zlocs
vsnap_z : [-300, 0, 300]  # z-locations for phsse-space snapshots.

# To capture the evolution of the field variables for given modes over the entire domain.
n_zsnap     : 5              # snapshot of entire domain for the v_modes at zsnap_vmodes
                             # between time = 0 and time = end_time
zsnap_v : [-1, -0.5, 0.5, 1] # velocity modes for full spatial domain snapshots.

#----------------------------------------------------------------#

# Folder names
folder_fv : "output-from-fv"
folder_fd : "output-from-fd"

#----------------------------------------------------------------#

```
We can now run the command `python manage.py --mcore fd` and `python manage.py --mcore fv` from the `COSEnu` folder to compile the jobs with finite difference and finite volume methods respectively. The output of these commands will look somethig as follows.

`python manage.py --mcore fv`:

```
[ OK ]...Creating /COSEnu/output-from-fv
[ OK ]...Created /COSEnu/output-from-fv/5000_64_0.2
[ OK ]...Created /COSEnu/output-from-fv/5000_64_0.4
[ OK ]...Created /COSEnu/output-from-fv/5000_128_0.2
[ OK ]...Created /COSEnu/output-from-fv/5000_128_0.4
[ OK ]...Created /COSEnu/output-from-fv/10000_64_0.2
[ OK ]...Created /COSEnu/output-from-fv/10000_64_0.4
[ OK ]...Created /COSEnu/output-from-fv/10000_128_0.2
[ OK ]...Created /COSEnu/output-from-fv/10000_128_0.4
[ OK ]...Created /COSEnu/output-from-fv/20000_64_0.2
[ OK ]...Created /COSEnu/output-from-fv/20000_64_0.4
[ OK ]...Created /COSEnu/output-from-fv/20000_128_0.2
[ OK ]...Created /COSEnu/output-from-fv/20000_128_0.4
/COSEnu/lib/main.o removed.
/COSEnu/lib/main removed.
g++ -O3 -std=c++0x -fopenmp  main.cpp -c -o main.o
g++ -O3 -std=c++0x -fopenmp main.o -o main 
main generated for multicore job
```


`python manage.py --mcore fd`:

```
[ OK ]...Creating /COSEnu/output-from-fd
[ OK ]...Created /COSEnu/output-from-fd/5000_64_0.2
[ OK ]...Created /COSEnu/output-from-fd/5000_64_0.4
[ OK ]...Created /COSEnu/output-from-fd/5000_128_0.2
[ OK ]...Created /COSEnu/output-from-fd/5000_128_0.4
[ OK ]...Created /COSEnu/output-from-fd/10000_64_0.2
[ OK ]...Created /COSEnu/output-from-fd/10000_64_0.4
[ OK ]...Created /COSEnu/output-from-fd/10000_128_0.2
[ OK ]...Created /COSEnu/output-from-fd/10000_128_0.4
[ OK ]...Created /COSEnu/output-from-fd/20000_64_0.2
[ OK ]...Created /COSEnu/output-from-fd/20000_64_0.4
[ OK ]...Created /COSEnu/output-from-fd/20000_128_0.2
[ OK ]...Created /COSEnu/output-from-fd/20000_128_0.4
/COSEnu/lib/main.o removed.
/COSEnu/lib/main removed.
g++ -O3 -std=c++0x -fopenmp  main.cpp -c -o main.o
g++ -O3 -std=c++0x -fopenmp main.o -o main 
main generated for multicore job
```

A folder for each job is created under the scheme directory and each job dirctory(eg. 5000_64_0.2) now contains a `job.config` file amd the executable(named `main` by default). The `job.config` contains the specifications and options for the given job and look like as given below.

```
scheme      : FV 
z0          : -600 
z1          : 600 
nvz         : 64 
CFL         : 0.2 
gz          : 4 
v0          : -1.0 
v1          : 1.0 
nz          : 10000 
N_ITER      : 50000 
ANAL_EVERY  : 500 
pmo         : 1.0 
omega       : 1.0 
theta       : 37 
mu          : 1.0 
n_fullsnap  : 3 
n_vsnap     : 5 
vsnap_z     : [-300, 0, 300] 
n_zsnap     : 5 
zsnap_v     : [-1, -0.5, 0.5, 1] 

```

Now to run the job,  example  with the ID 5000_64_0.2, we can `cd` to the job directory `5000_64_0.2` and execute the command

`./main --id 5000_64_0.2 --conf job.config`

As mentioned earlier, for `<ID>` (after `--id`) any valid string (even an empy string) should suffice, as it is used only to tag the output files. 