# Kernel from Flows  
Generate a kernel from flow fields. The dot-products are saved in a variable ``Phi`` in the resulting ``dp_*.mat`` file.

* **Flow fields** (select files)  
Select the flow fields for each subject.

* **Regularisation Form** (choose from the menu)  
The registration is penalised by some "energy" term.  Here, the form of this energy term is specified. Three different forms of regularisation can currently be used.

* **Reg params** (enter text)  
For *linear elasticity*, the parameters are $\mu$, $\lambda$ and id. For *membrane energy*, the parameters are $\lambda$, unused and id, where id is a term for penalising absolute displacements, and should therefore be small.  For *bending energy*, the parameters are $\lambda$, id$_1$ and id$_2$, and the regularisation is by $(-\lambda \nabla + \mathsf{id}_1)^2 + \mathsf{id}_2$

* **Dot-product Filename** (enter text)  
Enter a filename for results (it will be prefixed by ``dp_`` and saved in the current directory.
