# Running a simulation

## Creating the output folders and compiling

Once the nesessary setups and the initialization of the grid points are spcified in the configuration file (`COSEnu/lig/config.yaml`), we need to compile the code and make the necessary foldes to store the output files. In order to reduce the effort to manually doing all these, `COSEnu` provides a python script(`COSEnu/manage.py`), which reads the configuration file, makes the folders according to the specifucations on the configuration file and compiles the code according to the options provided. Python 3.0+ is preffered to run the script (anaconda version can be installed from the terminal using `wget <url>`). The general form of using the script is as follows. 

`$python manage.py [opt] [mode] [scheme]`

Available options for `[opt]` are:

-`--score` : Compiles the code for single core usage.

-`--mcore` : Compiles the code for multi-core usage.

-`--acc` : Compiles the code with `pgc++` to be used on GPU.

Available options for `mode` are:

`--ff` : Initialize the density matix componets and the angular distributions from previously stored binary file. This option is necessary only if we need to resume the simulation which has been stopped before finishing. Otherwise this option can be left unspecified.

Avilable options for `[scheme]` are: 

`--fd` : Compiles the code with finite difference scheme(3rd order Kreiss-Oliger dissipation with $$ \var{\epsilon}_\mathrm{KO}=0.1 $$ is supported by default).

`--fv` : Compiles the code with finite volume scheme.

The sript will automatically copy the executable (named `main` by default) and configuration file(named `job.config` by default) to the corresponding folders (created automatically according to the specifications given in the `config.yaml` file). 

Finally run the command

`./main --id <ID> --config job.config`

from the job folder to run the executable. The value specified for `<ID>` can be any acceptable string and will only be used for labeling the output files.  

[<previous](usage.md)  &#124;  [home](index.md)  &#124;  [next>](example.md) 
