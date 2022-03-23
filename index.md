<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Contents

1. [About](index.md)
2. [Theoretical framework](index.md)
3. [Numerical schemes](comp_setup.md)
4. [Setting-up a simulation](usage.md)
5. [Running a simulation](run.md)
6. [Example](example.md)
7. [Contributors](contributors.md)

# About

Collective Oscillation Simulation Engine for Neutrinos -- `COSE`$$\nu$$ is written completely in `C++` and provides two advanced numerical schemes
to simulate collective neutrino oscillations in the mean-field limit. The first method uses fourth order central finite differencing supplimented by third 
order Kreiss-Oliger dissipation scheme. The second one is implemented with the finite volume method along with the seventh order weighted 
essentially non-oscillatory scheme for the flux reconstruction across the cell boundaries. In both cases the time evolution is carried out via fourth order Runge-kutta method. 

# Theoretical framework

`COSE`$$\nu$$ solves the following 1-D hyperbolic equation with a source term which describes the evolution of a two-flavor neutrino system.

$$
\begin{equation}
\left( \frac{\partial}{\partial t} + \mathrm{v}\frac{\partial}{\partial z}\right)\rho_\mathrm{v}(z,t) = -i\bigg[H_\mathrm{v}(z, t), \rho_\mathrm{v}(z, t)\bigg]
\end{equation} ~~~~~~ (1) 
$$

 $$\rho_\mathrm{v}(z, t)$$ is a complex valued $$2\times2$$ matrix carrying the information about the number densities (diagonal entries) of the e-type 
 ($$\nu_e$$) and the x-type ($$\nu_x$$) neutrinos, and the correlations (off-diagonal entries) among them for a given velocity mode $$v$$ at 
 position $$z$$ and time $$t$$. The density matrix $$\rho_\mathrm{v}$$ takes the following form.

 $$
\rho_\mathrm{v}(z, t) = \left(\begin{align}
&\rho_{ee} ~~ \rho_{ex} \\
&\rho_{ex}^* ~~ \rho_{xx}
\end{align}
\right). ~~~~~~ (2)
$$

The quantity $$H_\mathrm{v}(z, t)$$ on the right hand side of the equation (1) represents the Hamiltonian which dictates the dynamics of the flavor transitions. 
In general, $$H_\mathrm{v}$$ contentains contributions from vacuum mixing $$H^{\text{vac}}$$, interaction with matter $$H^{\text{m}}$$ and the interactions 
among themselves $$H^{\nu\nu}$$. In the present implementation of `COSE`$$\nu$$, the contribution from matter has been neglected. Thus, $$H_\mathrm{v}$$ takes the following form,


$$
\begin{align}
H_\mathrm{v}(z, t) &= H^{\text{vac}} + H^{\nu\nu} \\
&= H^{\text{vac}} + \mu\int_{-1}^{1}d\mathrm{v}'(1-\mathrm{v}\mathrm{v}')\left(\rho_\mathrm{v'}-\bar{\rho}_\mathrm{v'}^*\right), ~~~~~~ (3)
\end{align}
$$

where $$\bar\rho$$ is the density matrix for antineutrinos.


[<previous]() &#124; [home](index.md) &#124; [next>](comp_setup.md)
