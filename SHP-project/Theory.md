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

### Computational Algorithms Required

The default numerical method used for numerically solving the Hamiltonian is called with SciPy by the `eigsh` method, and is a wrapper for the ARPACK function, which uses the **Implicitly Restarted Lanczos Method**[^1] to find the eigenvalues and eigenvectors [^2][^3]. The Lanczos method is an orthogonal projection method. The `eigsh` function finds \( k \) eigenvalues and eigenvectors of a real symmetric square matrix or a complex Hermitian matrix \( A \), and this method for diagonalization is often preferred for large sparse matrices[^4].

ARPACK can solve either standard eigenvalue problems of the form:

$$
Ax = \lambda x
$$

or general eigenvalue problems of the form:

$$
Ax = \lambda Mx
$$

One particular limitation of ARPACK is that it is generally better at finding **extremal eigenvalues** (eigenvalues with very large magnitudes) [^5]. Therefore, this can lead to anomalous results and very slow execution time, especially in contexts like investigating the potentials of Si and AgCrS\(_2\), where **low eigenvalues** are required.

To address this, the default solver method `eigsh` uses a **shift-invert trick** for improved stability when finding low-lying states [^6]. This technique leverages the fact that, for a generalized eigenvalue problem \( Ax = \lambda Mx \), it can be reformulated as:

$$
(A - \sigma M)^{-1} M x = \nu x
$$

where

$$
\nu = \frac{1}{\lambda - \sigma}
$$

By implementing the shift-invert trick in the `eigsh` solver, the Hamiltonian can be diagonalized without requiring extensive computational resources, allowing the return of eigenstates for the input anharmonic potential.

---

[^1]: See reference [Lanczos method] for detailed information on the Hamiltonian eigenvalue problem.  
[^2]: SciPy `eigsh` documentation  
[^3]: Hamiltonian diagonalization methods  
[^4]: A sparse matrix is defined as a matrix in which very few elements are non-zero.  
[^5]: ARPACK documentation  
[^6]: qmsolve documentation


[^1]: qmsolve documentation  
[^2]: Numerically Solving the Schrödinger Equation (source)
