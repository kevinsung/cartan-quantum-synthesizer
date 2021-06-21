This code implements the algorithm(s) found in:

Fixed Depth Hamiltonian Simulation via Cartan Decomposition
Efekan Kökcü, Thomas Steckmann, J. K. Freericks, Eugene F. Dumitrescu, Alexander F. Kemper
https://arxiv.org/abs/2104.00728

_Abstract_:
Simulating spin systems on classical computers is challenging for large systems due to the significant memory requirements. This makes Hamiltonian simulation by quantum computers a promising option due to the direct representation of quantum states in terms of its qubits. Standard algorithms for time evolution on quantum computers require circuits whose depth grows with time. We present a new algorithm, based on Cartan decomposition of the Lie algebra generated by the Hamiltonian, that generates a circuit with time complexity O(1) for ordered and disordered models of n spins. We highlight our algorithm for special classes of models where an O(n^2)-gate circuit emerges. Compared to product formulas with significantly larger gate counts, our algorithm drastically improves simulation precision. Our algebraic technique sheds light on quantum algorithms and will reduce gate requirements for near term simulation.

Authors:
  Efekan Kökcü
  Thomas Steckmann

# Documentation
Please See the [Documentation](docs/) for Information on the Functions included in the package. Documetation generated by pdoc in the google style. A google style reference document is included. 

To recompile the documentation, run 
`pdoc -d google -o .\docs .\package` 
from the main folder (requires pdoc--not pdoc3 or pdocs) 

# General Notes:
PauliStrings are formatted as one of the following:
* Text based: AKA 'PauliString' formats the Pauli String σ<sub>x</sub>⦻σ<sub>y</sub>⦻σ<sub>z</sub>⦻σ<sub>I</sub>⦻σ<sub>I</sub> as `XYZII`
* Tuple Bases: AKA (PauliString) formats σ<sub>x</sub>⦻σ<sub>y</sub>⦻σ<sub>z</sub>⦻σ<sub>I</sub>⦻σ<sub>I</sub> as `(1,2,3,0,0)` where each 0,1,2,3 referes the the standard index of the Pauli matricies: 

0 -> I, 1 -> X, 2-> Y, 3-> Z
# Example Usage:
The functionality is based on three object classes:
<ol>
    <li> <code>Hamiltonian</code>: The Hamiltonian object is used to construct information about the model that you wish to simulate. There are predefined models that can be used for example implementation, the user can define a custom Hamiltonian and pass it to the object, the user can combine existing Hamiltonians to generate the desired Hamiltonian (ex. XY + transverse field), or the user can define their own input. This is designed to be relatively straightforward, and the other definitions can serve as examples </li>
    <li> <code>Cartan</code>: The Cartan object is used to define a cartan decomposition on the "Hamiltonian Algebra" generated by Hamiltonian object. The "Hamiltonian Algebra, g(H) or g, is defined as the set of Pauli Strings that can be generated through nested commutators on the Hamiltonian. Thus, g(H) is a closed Lie algebra. We can then perform a Cartan decomposition, which splits g into two components, k and m, based on some involution. The Cartan object has a list of predefined Cartan involutions, although users are encouraged to add an name their own implementations. Finally, the Hamiltonian algebra has as Cartan Subalgebra h, which is a maximal abelian subalgebra that is contained within the m partition. This is generated by the object.  After generated the Cartan object, the involution (k and m) can be altered, and h can be regenerated based on a list of commuting terms in m. h is generally not unique. </li>
    <li> <code>FindParameters</code>: The FindParameters object optimizes the terms in the cartan decomposition to generate a time evolution circuit for the Hamiltonian. For details of the theory, please see the paper. The Find Parameters object relies on a cost function formulated by Earp and Pachos and improved in the original paper by Kokcu et. al. The Find Parameters object takes a filename, a Cartan object (which contains a Hamiltonian object), and allows for changes in the optimizer function (ex. BFGS gradient decent or Powell), or the optimization accuracy. Warning: Runtime scales quickly with system size. </li>
</ol>
For example, to use these objects to generate the decomposition for the time evolution of the four site 1D lattice XY model, the code would be as follows:

### Import Key elements of the package:
`
from package.src.Hamiltonian import Hamiltonian
from package.src.Cartan import Cartan
from package.src.FindParameters import FindParameters
`

### Define the parameters of the model:
```
#Define the number of lattice points
sites = 4 

#Define the model information. This is an open boundary condition 1D lattice with the XY model. Formatted as [(Coefficient, 'ModelName'), closedBoundaryCondition)]
model = [(1,'xy', False)] 
`
The Hamiltonian would be: `1*XXII + 1*YYII + 1*IXXI + 1*IYYI + 1*IIXX + 1*IIYY`. 


### Create the Hamiltonian Object
`
xy = Hamiltonian(sites,model)
`

### Define the Cartan Object
`
xyC = Cartan(xy)
`
The default Involution is Even-Odd, which counts the number of X and Y terms in each component of the algebra. 

### Run the FindParameters object
`
xyP = FindParameters(xyC)
xyP.printResult()
`
# Current State:
Version 0.1 - Currently stable, requires some bug fixing. If you find an error in the code, please contact Thomas Steckmann @tmsteckm or Efekan Kokcu

## TODO: 
 * Fix Documentation
 * Implement saving and loading
 * Hubbard Model methods
 * Choose decomposition default based on model

