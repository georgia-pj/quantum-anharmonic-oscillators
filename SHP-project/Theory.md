### Numerically Solving the Schrödinger Equation

The Schrödinger equation is a partial differential equation, known as a wave equation, which is a mathematical model used to predict the probability of an outcome. The non-relativistic, time-independent Schrödinger equation for the structure of a system is defined as:

$$
H \psi(x) = E \psi(x)
$$

In this equation, the kinetic and potential energies of the system are transformed into the Hamiltonian \( H \), which acts upon the wavefunction \( \psi(x) \).

The `qmsolve` module used for this project takes a potential function as input and returns the corresponding energies and eigenstates. It does this by constructing the Hamiltonian as a matrix, where the Hamiltonian is the sum of the potential energy (representing the potential without the effects of momentum terms) and the kinetic energy of the system [^1].

To facilitate numerical computation, the continuous spatial coordinates are discretized into a finite number of points. The code discretizes spatial coordinates \( x \) into \( N \) points with step size \( \delta x \). Then, the wavefunction is represented as a column vector \( \psi \) with \( N \) components, where each component corresponds to the wavefunction at a specific grid point [^2].

The Hamiltonian is constructed by discretizing each term in the Hamiltonian using finite differences and combining them into a matrix. The Hamiltonian is defined as the sum of the kinetic and potential energies of the quantum system:

$$
H = -\frac{\hbar^2}{2m} \frac{\delta^2}{\delta x^2} + V(x)
$$

The second derivative term in the Hamiltonian can be approximated using finite differences:

$$
\nabla^2 \psi(x) \approx \frac{\psi(x+\Delta x) - 2\psi(x) + \psi(x - \Delta x)}{\Delta x^2}
$$

Using this approximation, the discretized Schrödinger equation can be expressed in matrix form as:

$$
-\frac{\hbar^2}{2m} \nabla^2 \psi + V(x) \psi = E \psi
$$

The Hamiltonian is then diagonalized to retrieve the lowest-energy eigenstates using the `eigsh` method.

---

[^1]: qmsolve documentation  
[^2]: Numerically Solving the Schrödinger Equation (source)
