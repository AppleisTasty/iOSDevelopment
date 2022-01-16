1. List the three main components in a query processor.



2. Describe the three main issues in query optimization



3. Explain why map-reduce computations can directly express queries of the form: SELECT FROM WHERE GROUP BY



5. **Articulate** the complementarity (at least two items) of SQL and NoSQL systems.

> SQL: 
>
> 1. data intergrity：数据完整性，通过schema加强
> 2. strict transaction semantics：防止并行程序形成 inconsistency
>
> NoSQL: 
>
> 1. Elastic scaling：体积易于成长（应对web庞大的数据）
> 2. Simple operations：操作要方便



6. Contrast ACID guarantees in SQL databases and BASE guarantees in NoSQL databases.



7. Given a RA algebraic expression, for each non-leaf node in the logical operator tree, sketch the pseudocode of the mapper and the reducer functions that would compute the correct value for the algebraic expression.



8. Compare and contrast MapReduce, DataFlow Engines to Massively Parallel Processing(MPP) Databases.



9. Briefly explain why the triple store strategy for mapping RDF data into relational data creates obstacles for efficient evaluation.

week4 - slides79

With a single table, queries will often require multiple self-joins, which are, in principle, often difficult to evaluate efficiently.

10. 

$\pi _{B.name, B.address, D.name, D.address} (\sigma _{F.times\_a\_week > 1} (\rho _D (Drinker) \bowtie _{D.name = F.drinker} \rho _F (Frequents) \bowtie _{F.bar = B.name} \rho _B (Bar)))$

$\{A| \exists D \in Drinker \space \exists F \in Frequents \space \exists B \in Bar( F.times\_a\_week > 1 \and D.name = F.drinker \and F.bar = B.name \and A.bname = B.name \and A.baddress = B.address \and A.pname = D.name \and A.paddress \ D.address) \}$

