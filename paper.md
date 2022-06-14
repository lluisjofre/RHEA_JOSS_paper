---
title: 'RHEA: an open-source Reproducible Hybrid-architecture flow solver Engineered for Academia'
tags:
  - Compressible flow
  - Fluid mechanics
  - Ideal- & real-gas thermodynamics
  - Turbulence
  - C++
  - MPI
  - OpenACC
  - HDF5
  - YAML
authors:
  - name: Lluís Jofre
    orcid: 0000-0003-2437-259X
    affiliation: 1
  - name: Ahmed Abdellatif
    orcid: 0000-0002-8810-7480
    affiliation: 1
  - name: Guillermo Oyarzun
    orcid: falta ...
    affiliation: 2
affiliations:
 - name: Dept. Fluid Mechanics, Technical University of Catalonia - BarcelonaTech, Barcelona 08019, Spain
   index: 1
 - name: falta ...
   index: 2
date: 11 July 2022
bibliography: paper.bib
---

# Summary

# Statement of need

# Flow solver design

# Computational performance

# Application example
The ability of RHEA to easily configure computational flow problems and run them efficiently on top-tier supercomputing systems is demonstrated by simulating the canonical three-dimensional (3-D) turbulent channel flow problem [@Smits2011-A] on the CTE POWER9 cluster of the Barcelona Supercomputing Center (BSC) [@BSC]. The friction Reynolds number selected is $Re_{\tau} = u_{\tau} \delta / \nu = 180$, where $u_{\tau} = 1\thinspace\textrm{m/s}$ is the friction velocity, $\delta=1\thinspace\textrm{m}$ is the channel half-height, and $\nu = \mu / \rho$ is the kinematic viscosity of the fluid with $\mu$ the dynamic viscosity and $\rho = 1\thinspace\textrm{kg/m}^\textrm{3}$ the density. The Prandtl number of the problem is $Pr = c_p \mu/\kappa = 0.71$ with $c_p$ the isobaric specific heat capacity, and the Mach number is $Ma = u_b/\sqrt{\gamma P_b/\rho} = 0.3$ with $u_{b}$ and $P_b$ the bulk velocity and pressure, respectively, and $\gamma=1.4$ the heat capacity ratio. The mass flow rate in the streamwise direction is imposed through a body force equal to $\textbf{f}=\left[\tau_{w}/\delta,0,0\right]^{\intercal}$, where $\tau_{w}$ is the wall shear stress.

The computational domain is $4 \pi \delta \times 2\delta \times 4/3\pi \delta$ in the streamwise ($x$), wall-normal ($y$), and spanwise ($z$) directions, respectively. The streamwise and spanwise boundaries are set periodic, and no-slip conditions are imposed on the horizontal boundaries ($x$-$z$ planes). The grid is uniform in the streamwise and spanwise directions with resolutions in wall units equal to $\Delta x^{+} = 9$ and $\Delta z^{+} = 6$, and stretched toward the walls in the vertical direction with the first grid point at $y^{+} = y u_{\tau}/\nu =0.1$ and with sizes in the range $0.1 \lesssim \Delta y^{+} \lesssim 4$. This grid arrangement corresponds to a direct numerical simulation (DNS) of size $256 \times 128 \times 128$ grid points. The simulation strategy starts from a linear velocity profile with random fluctuations as proposed by Nelson and Fringer [@Nelson2017-A], which is advanced in time utilizing the KGP convection scheme [@Coppola2019-A] with $\textrm{CFL}=0.9$ to reach turbulent steady-state conditions after approximately $20$ flow-through-time (FTT) units; based on the bulk velocity $u_{b}$ and the length of the channel $L_x = 4\pi\delta$, a FTT is defined as $t_{b} = L_x/u_{b} \sim \delta/u_{\tau}$. Flow statistics are collected for roughly $30$ FTTs once steady-state conditions are achieved, and compared against reference results from Moser et al. [@Moser1999-A]. The simulation is run on 2 IBM POWER9 nodes, each containing 2 Witherspoons CPUs (20 cores of 3.1 GHz each), 4 Volta NVIDIA GPUs (16 GB each) and 6.4 TB of NVMe (Non-Volatile Memory). 

\begin{figure}
 \centering
 \subfloat[]{\includegraphics[width=0.490\textwidth]{u_plus_vs_y_plus.eps}}
 \subfloat[]{\includegraphics[width=0.500\textwidth]{uvw_rms_plus_vs_y_plus.eps}}
 \caption{Comparison against Moser et al. [@Moser1999-A] of the time-averaged streamwise velocity $u^+$ (a) and $\textrm{rms}$ velocity fluctuations $u_{\textrm{rms}}^{+}$, $v_{\textrm{rms}}^{+}$ and $w_{\textrm{rms}}^{+}$ (b) along the wall-normal direction $y^+$ in wall units.}  \label{fig:3d_turbulent_channel_flow_statistics}
\end{figure}

The time-averaged mean streamwise velocity $u^+$ and root-mean-squared ($\textrm{rms}$) velocity fluctuations $u_{\textrm{rms}}^{+}$, $v_{\textrm{rms}}^{+}$, $w_{\textrm{rms}}^{+}$ along the wall-normal direction $y^+$ in wall units provided by RHEA and compared to the reference results from Moser et al. [@Moser1999-A] are depicted in Fig.~\ref{fig:3d_turbulent_channel_flow_statistics}. As shown in the figure, the results from RHEA accurately reproduce the first- and second-order turbulent flow statistics by (i) properly capturing the inner (viscous sublayer, buffer layer and and log-law region) and outer layers in Fig.~\ref{fig:3d_turbulent_channel_flow_statistics}(a), and (ii) the turbulent flow fluctuations peaking around $y^+\approx 15$ for the streamwise velocity in Fig.~\ref{fig:3d_turbulent_channel_flow_statistics}(b). Additionally, a snapshot of the instantaneous streamwise velocity in wall units $u^+$ on a $x^+$-$y^+$ slice is displayed in Fig.~\ref{fig:3d_turbulent_channel_flow_contours} to provide qualitative information of the wall-bounded turbulence at $Re_\tau = 180$ computed by RHEA.

\begin{figure}
 \centering
 \includegraphics[width=0.95\textwidth]{channel_u_velocity_vs_x_y_direction.png}
\vspace{-3mm}
 \caption{Snapshot of the instantaneous streamwise velocity in wall units $u^+$ on a $x^+$-$y^+$ slice from RHEA.}  \label{fig:3d_turbulent_channel_flow_contours}
\end{figure}

# Acknowledgements
This work is supported by the European Research Council (ERC) under the European Union’s Horizon Europe research and innovation programme (grant agreement No. 101040379 - SCRAMBLE), and the *Beatriz Galindo* programme (Distinguished Researcher, BGP18/00026) of the *Ministerio de Ciencia, Innovación y Universidades* (Spain).

# References
