<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Numerical setup

In order to solve the equation (1) numerically, we devide the spatial domain spanning from $$z_0$$ to $$z_1$$ into
$$N_z$$ discreate points an such that the size of each cell $$dz = (z_1 - z_0)/N_z$$. Furthermore we also devide
the velocity domain into $$N_{v_z}$$ bins such that the integration in the equation (3) and evolution of each
velocity mode can be effectively carried out.


With the above discretization, the initial value problem in the equation (1) can be transformed into the flollowing form.
      
      
$$
\frac{d\rho_{\mathrm{v},i}}{dt} = L^{\text{vac}}_i[\rho_\mathrm{v}] + L^{\nu\nu}_i[\rho_\mathrm{v}] + L^{adv}_i[\rho_\mathrm{v}]
$$
     

     
where, $$L_i^\text{vac}, L_i^{\nu\nu}$$ and $$L_i^\text{adv}$$ are the operators which computes the contribution from vacuum 
oscillation, neutrino-neutrino interaction and the spatial derivative respectively at the $$i^\text{th}$$ cell, $$0\leq i < N_z$$. 
Thus given the values of the field variables at $$t=0 $$ of each velocity mode at all grid points, the successive states of the 
field varibles can be solved using standard ordinary differential equation solving techniques such as Runge-Kutta method. 

## Finite difference (FD) implementation details

### The pseudo code for the FD scheme
```
NuOsc::calRHS(FieldVar *out, const FieldVar *in){
    for all velocity bins{
        for all spatial grids{
    
        #ifdef VAC_OSC_ON
            out += -i [H_vac, in]
        #endif

        #ifdef ADV_FD
            out += L_adv[in]
        #endif

        #ifdef COLL_OSC_ON 
        for all velocity bins{
            do the computation within the integral
        }
        #endif

        #ifdef KO_FD
            do the Kreiss-Oliger dissipation
        #endif
        }
    }
}
```
## Finite volume (FV) implementation details

Although most of the part of the implementation of $$\text{COSE}\nu$$ with FV remains same as that of FD, the FV treatment considers
the field variables as cell averaged quantities. As a result, the spatial derivative at $$i^\text{th}$$ grid point become proportional 
to the difference in fluxes across the boundaries of the same cell. For our 1-D case, these boundaries are located at $$(i-1/2)*dz$$ and
$$(i+1/2)*dz$$. Thus we have,

$$
\left(\mathrm{v}\frac{\partial\rho_\mathrm{v}(z, t)}{\partial z}\right)_{z=i*dz} \rightarrow
\frac{1}{dz}\left(\mathcal{F}_{i+1/2}[\tilde\rho_\mathrm{v}]-\mathcal{F}_{i-1/2}[\tilde\rho_\mathrm{v}]\right)
$$


where $$\mathcal{F}$$ is the flux function associated with the cell averaged quantity $$\tilde\rho\_mathrm{v}$$.
There are different approximations by which we can estimate the flux function. Here we adopted the
seventh order accurate Weighted Essentially Non-Oscillatory (WENO7) scheme with 3 fourth order accurate
sub-stencils for the flux reconstruction at the cell boundaries. In this case the flux function can be
written as,

$$
\mathcal F^7_{i+1/2} = \sum_{r=0}^2 w_r \mathcal{F}^4_{r, i+1/2}
$$

$$w_r$$ here is the weight factor for the fourth order accurate flux function constructed from the sub-stencil indexed using the shift parameter $$r$$.

### The pseudo code for the FV scheme

```
NuOsc::calRHS(FieldVar *out, const FieldVar *in){
    compute flux using WENO7
    for all velocity bins{
        for all spatial grids{
            out = 0
        #ifdef ADV_FV
            out += net_flux(in) // calculated using WENO7
        #endif

        #ifdef VAC_OSC_ON
            out += -i [H_vac, in]
        #endif

        #ifdef COLL_OSC_ON 
        for all velocity bins{
            do the computation within the integral
        }
        #endif
    }
}

```

[<previous](index.md) [next>](usage.md)