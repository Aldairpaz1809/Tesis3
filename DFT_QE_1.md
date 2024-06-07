# __DFT y Quantum Espresso Tutorial__ 

Notas sobre Quantum Espresso y Teoría de la funcional de la densidad.<br>
**Índice**
1 .[Fudamento Teórico](#id1)
##  __1. Fundamento Teórico__<a name="id1"></a>

Para resumir el fundamento teórico necesario para comprender la Tería funcional de la densidad. Se resumiran conceptos básicos y definiciones necesarias al inicio.

### __1.1 Ecuación de Schrödinger__

En mecánica cuántica, la ecuación que gobierna el compotamiento de las partículas es la ecuación de Schrödinger, la cual es:
$$Ĥ~\Psi(\mathbf{\vec{r}},t)=\frac{d\Psi}{dt}(\mathbf{\vec{r}},t)$$

Normalmente se trabaja la ecuación de Schrödinger independiente del tiempo, debido a que el estado base del sistema es independiente del tiempo, donde el autovalor del operador de Hamilton, $Ĥ$, es la energía del sistema:
$$ Ĥ~\Phi(\mathbf{\vec{r}})=E~\Phi(\mathbf{\vec{r}})$$

> __NOTA:(Del curso de _[Computational Materials Physics](https://www.compmatphys.org/)_ )__ <br>Se conoce como Ab Initio o primeros principios a los estudios de la naturaleza basandose en ecuaciones fundamentales (e irreductibles) que describen completamente al sistema estudiad. Por ejemplo, en mecánica clásica, usar las ecuaciones de Newton se consideraría un estudi Ab Initio. En este aspecto, se trata de evitar modelos que simplifiquen en desmedida al sistema a estudiar.

Ahora consideremos un sistema de cargas negativas y positivas (por ejemplo, una molécula de N nucleos y M electrones), su ecuación de Schrödinger está dada por:
$$ Ĥ~\Phi(\mathbf{\vec{r}},\mathbf{\vec{R}})= \left( \sum_{A}^{N}\frac{\hbar^2}{2M_A} \nabla_A^2 + \frac{\hbar^2}{2m}\sum_i^M\nabla_i^2+\sum_i^N \sum_{j<i}^N\frac{1}{r_{ij}}+\sum_A^M \sum_{B<A}^M\frac{Z_AZ_B}{r_{AB}}+\sum_i^N \sum_{A}^M\frac{Z_A}{|R_A-r_i|} \right)\Phi(\mathbf{\vec{r}},\mathbf{\vec{R}})=E\Phi(\mathbf{\vec{r}},\mathbf{\vec{R}}) $$

Los primeros 2 términos se refieren a la energía cinética de las cargas ($Z_A$ se refiere al número de cargas positivas en los núcleos atómicos). Las letras mayusculas hacen referencia a las cargas postivas y las minúsculas a las cargas negativas.

### __1.2 Aproximación Bohr-Oppenheimer__

En general, podriamos considerar que las cargas positivas (en muchos casos compouestas por protones y neutrones agrupados en nucleos atómicos mediante la fuerza fuerte) se mueven a una velocidad mucho menor que las cargas negativas (electrones) debido a que poseen una masa mucho mayor. Esto implica que en el tiempo en el que las cargas positivas cambian de estado, las cargas negativas ya se han reconfigurado a su nuevo estado; entonces para un dado periodo de tiempo lo sificientemente pequeño podemos considerar la energía cinética de las cargas positivas como una constante (es decir, que tienen una posición fija); al igual que el potencial debido a la atracción entre las cargas positivas. Con esto podemos escribir el Hamiltoniano electrónico que solo toma en cuenta a la enerǵia cinética de los electrones, su potencial de atracción entre los electrones y el potencial:

$$  Ĥ_e~\Phi_e(\mathbf{\vec{r}},\mathbf{\vec{R}})= \left( \frac{\hbar^2}{2m}\sum_i^M\nabla_i^2+\sum_i^N \sum_{j<i}^N\frac{1}{r_{ij}}+\sum_i^N \sum_{A}^M\frac{Z_A}{|\mathbf{\vec{R}_A-\vec{r}_i}|} \right)\Phi_e(\mathbf{\vec{r}},\mathbf{\vec{R}})=E_e\Phi_e(\mathbf{\vec{r}},\mathbf{\vec{R}})$$
$$Ĥ=E_e+V_{ext}$$
$$E_e=T_e+V_e $$

### __1.3 Producto de Hartree__

Podemos aproximar la función de onda electrónica en un producto de N funciones, cuyo argumento corresponde a las coordenadas de los N electrones: $\Phi_e=\Phi (\mathbf{\vec{r}_1})\Phi (\mathbf{\vec{r}_2}) \cdots\Phi (\mathbf{\vec{r}_N})$. A esto se le conoce como producto de Hartree; con esto podemos analizar el problema como varias funciones de ondas individuales de los electrones.

### __1.2 Teoría Funcional de la densidad__<a name="id1"></a>

### __1.2.1 Teoremas de Hohenberg y Kohn__

La formulación tradicional de los dos teoremas de Hohenberg y Kohn como sigue:
>__Primer teorema__: Existe una _correspondencia de uno a uno_ entre la densidad de estados en el estado base $\rho (\vec{r})$ de un sistema de muchos electrones (átomos, moléculas, solidos,...) y el potencial externo $V_{ext}$.<br>

Una consecuencia inmediata es que el valor esperado en el estado base de cualquier observable $Ô$ es un _funcional único cuyo argumento es la densidad de estados en el estado base_:
$$<\Psi|\^{O}|\Psi>=\^O [\rho]$$
donde $\^{\rho}(\vec{r})=\int_V \Psi _0^*\Psi _0 d\vec{r}^3=\sum_{i=1}^N\delta (\vec{r}_i-\vec{r})$ (considerando el producto de Hartree),entonces:

$$\rho(\vec{r})=<\Psi(\vec{r}_1,\vec{r}_2,\dots,\vec{r}_N)|\^{\rho}(\vec{r})|\Psi(\vec{r}_1,\vec{r}_2,\dots,\vec{r}_N)> ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~$$

$$=<\Psi(\vec{r}_1,\vec{r}_2,\dots,\vec{r}_N)|\sum_{i=1}^N \delta (\vec{r}_i-\vec{r})|\Psi(\vec{r}_1,\vec{r}_2,\dots,\vec{r}_N)> ~~~~~~~~~~$$

$$=\sum_{i=1}^N \int_V \Psi(\vec{r}_1,\dots, \vec{r_i}=\vec{r},\dots ,\vec{r}_N)^*\Psi(\vec{r}_1,\dots, \vec{r_i}=\vec{r},\dots ,\vec{r}_N) $$

>__Segundo Teorema:__ En particular, siendo $Ô$ el operador Hamiltoniano $Ĥ$, el funcional de la energía total de la energía base $H[\rho]=E_{V_{ext}}[\rho]$ es de la forma: 
$$E_{V_{ext}}[\rho] = \underbrace{<\Psi |\^{T}+\^{V}|\Psi>}_{F_{HK}[\rho]}+<\Psi|\^V_{ext}|\Psi>~~~$$
$$=F_{HK}[\rho]+\int \rho(\vec{r})V_{ext}(\vec{r})d\vec{r}$$ 
>Dónde el _Funcional de Densidad de Hohenberg-Kohn $F_{HK}[\rho]$_ is universal para cualquier sistema de muchos electrones. $E_{V_{ext}}[\rho]$ debe ser el mínimo posible  (cuya valor corresponde al de la energía total en el estado base) para la densidad de estados en el estado base correspondiente a $V_{ext}.$
### __1.2.2 Las ecuaciones de Kohn-Sham__
Las ecuaciones de Kohn Y Sham fueron publicadas en _1965_ convirtiendo a DFT en una herramienta práctica. Ellos publicaron una amnera práctica e obtener la densidad de estados base.

Reescribamos 

