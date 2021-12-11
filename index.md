<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# About
Collective Oscillation Simulation Engine for Neutrino-$$\text{COSE}\nu$$ is written completely in `C++` provide two advanced numerical schemes
to simulate collective neutrino oscillation in the mean-field limit. The first method uses fourth order accurate central differencing supplimented by third 
order Kreiss-Oliger error suppression scheme. The second one is implemented in using finite volume method along with the seventh order accurate weighted 
essentially non-oscillatory scheme for the flux reconstruction across the cell boundaries. In both cases time evolution is carried out via fourth order 
Runge-kutta method so that the spatial and temopral accuracies are of the same order.

# Theoretical setup

$$\text{COSE}\nu$$ solves the following 1-D hyperbolic equation with source term which describe the evolution of the two flavor neutrino system.

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
\right) ~~~~~~ (2)
$$

The quantity $$H_\mathrm{v}(z, t)$$ on the right hand side of the equation (1) represents the Hamiltonian which dictates the dynamics of the flavor transitions. 
In general $$H_\mathrm{v}$$ contentains contributions from vacuum oscillation $$H^{\text{vac}}$$, interaction with matter $$H^{\text{m}}$$ and the interactions 
among themselves $$H^{\nu\nu}$$. In the present implementation of $$\text{COSE}\nu$$, contribution from matter has been neglected assuming that the matter is homogeniously distributed, which therefore do not affect the neutrino flavor transitions. Thus, $$H_\mathrm{v}$$ takes the form,


$$
\begin{align}
H_\mathrm{v}(z, t) &= H^{\text{vac}} + H^{\nu\nu} \\
&= H^{\text{vac}} + \mu\int_{-1}^{1}d\mathrm{v}'(1-\mathrm{v}\mathrm{v}')\left(\rho_\mathrm{v'}-\bar{\rho_\mathrm{v'}}^*\right) ~~~~~~ (3)
\end{align}
$$

where, $$\bar\rho$$ is the density matrix for the anti-neutrino.


[<previous]()  [next>](comp_setup.md)



<!-- You can use the [editor on GitHub](https://github.com/Demon-of-Asgard/mypage/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Demon-of-Asgard/mypage/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out. -->
