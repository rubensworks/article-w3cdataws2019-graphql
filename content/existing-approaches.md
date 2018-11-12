## Existing Approaches
{:#existing-approaches}

To the best of our knowledge, four solutions exist for querying RDF via GraphQL queries:

<span class="comment" data-author="MVS">
For completeness, I'd add the maturity level + which solutions are enterprise and which onces are academic (also clarify that in the details, like with Stardog)
</span>

* [HyperGraphQL](cite:citesAsAuthority hypergraphql)
* [TopBraid](cite:citesAsAuthority topbraid)
* [Stardog](cite:citesAsAuthority stardog)
* [GraphQL-LD](cite:citesAsAuthority graphqlld)

The key properties of these solutions will be discussed hereafter.

### HyperGraphQL

[HyperGraphQL](cite:citesAsAuthority hypergraphql) is a GraphQL interface wrapper around RDF sources.
The main goal of this system is to support federated querying
over multiple RDF sources and make it accessible through a single interface.

Each HyperGraphQL instance uses a configuration file and an _annotated_ GraphQL schema.
The configuration file is used to define the RDF services to fetch data from.
The annotated GraphQL schema is a GraphQL schema that is annotated with GraphQL directives
that indicate for each type and argument in which RDF service the type can be fetched,
and what their full URI is.

Users can send plain GraphQL queries to an HyperGraphQL instance,
and the instance will reply with a GraphQL response that is enhanced with a [JSON-LD context](cite:citesAsAuthority jsonld).
This implicitly transforms the GraphQL response into RDF.
Optionally, the server provides different content types to convert the response into alternative RDF serializations.

### TopBraid

[TopBraid](cite:citesAsAuthority topbraid) is a set of semantic data governance tools.
In a recent release, GraphQL was added as a way to query the stored RDF.

TopBraid's approach is similar to HyperGraphQL.
It also uses GraphQL schemas to expose a GraphQL interface over RDF sources.
TopBraid's schemas are automatically generated based on data shape definitions,
which can be defined using [SHACL](cite:citesAsAuthority spec:shacl).

These schemas are more expressive than HyperGraphQL's schema,
as it supports things like SHACL-based constraint filters, full-text-search, aggregations, ordering and field transformations.
These features are defined via custom query functions in the schema.
In contrast to HyperGraphQL, TopBraid attaches no JSON-LD context to its GraphQL query responses.

### Stardog

[Stardog](cite:citesAsAuthority stardog) is a commercial graph store that recently
(as of release 5.3.3) adds GraphQL query support.

Stardog makes GraphQL schemas optional for querying RDF using GraphQL.
If no schema is used, then GraphQL term to RDF conversion will be done using the default namespace in that graph,
but this can be overridden by using the `@prefix` directive inside GraphQL queries.

Stardog assumes that the top-level node of any query is assumed to refer to a type.
Any deeper query nodes refer to predicate links from the parent node.

Stardog also adds several features to the GraphQL query side.
Compared to TopBraid, Stardog uses _directives_ for this, instead of dedicated arguments.
It adds support for functionality such as filters and bindings.

Just like TopBraid, no JSON-LD context is attached to its GraphQL query responses.

### GraphQL-LD

Finally, [GraphQL-LD](cite:citesAsAuthority graphqlld) is an approach
that is able to convert GraphQL queries to [SPARQL](cite:cites spec:sparqllang),
and SPARQL results to the corresponding GraphQL query results,
independent of a query engine.
As an example, GraphQL-LD was implemented in the [Comunica engine](cite:cites comunica),
and can be [tried out via a Web interface](https://gist.github.com/rubensworks/9d6eccce996317677d71944ed1087ea6){:.mandatory}.
Compared to the previously explained approaches, it does not use the GraphQL schema at all,
but exploits [JSON-LD contexts](cite:cites jsonld) to handle the conversion of GraphQL terms to RDF terms.
This offers a separation of concerns where developers can use simple GraphQL queries,
and domain experts can offer the JSON-LD context which _semantifies_ these queries.

GraphQL-LD also supports custom features such as filtering and ordering through directives.

GraphQL query responses in this approach are plain JSON objects,
and are compatible with the user-provided JSON-LD context.
As such, responses are usable as both JSON and RDF.
