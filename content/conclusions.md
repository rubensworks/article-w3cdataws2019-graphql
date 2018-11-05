## Conclusions
{:#conclusions}

In the previous section, we provided an overview of four approaches for querying RDF with GraphQL queries.
These approaches are significantly different and mostly incompatible with each other.
As such, one GraphQL query that works for one approach
will most likely not work in the other approaches,
or it can have different semantics.

Therefore, it is important to achieve a better understanding of the different ways of querying RDF with GraphQL.
Ideally, and if possible, a consensus should reached on a single _good way_.
In order to do this, we propose the following plan of action:

1. Determine the different aspects of querying RDF with GraphQL.
2. Decompose and categorize the methods of the existing approaches in these aspects.
3. Determine the advantages and disadvantages of each method within each aspect.
4. Propose a standard approach and semantics for querying RDF with GraphQL.

Clarifying the differences and a path to unification will
lower the barrier for developers that have knowledge on GraphQL to start querying RDF.
