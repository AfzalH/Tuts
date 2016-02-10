# Creating Mondrian Schema

Here's the step by step guide for creating a simple schema.

A Schema Should be defined under the `Schema` tag

```xml

<Schema name="HRM" description="Mondrian Schema for HRM">
</Schema>

```

Inside the Schema tag we can define our cubes. At the moment of writing we have only one cube

```xml

<Cube name="SanctionedPostCube" visible="true" description="Cube for sanctioned post" cache="true" enabled="true">
</Cube>

```

So the combined code is

```xml

<Schema name="HRM" description="Mondrian Schema for HRM">
	<Cube name="SanctionedPostCube" visible="true" description="Cube for sanctioned post" cache="true" enabled="true">
	</Cube>
</Schema>

```

A cube must contain a table from which the Measures/Values will be derived. It can then refer to other tables via foreign key for grouping those Measures/Values against those keys (grouped by those keys). For simplicity and performance improvement, we have joined multiple tables together to create one large table so we don't need to refer to other tables at this point.

A Table tag (goes inside Cube tag) should be defined as

```xml

<Table name="rep1_sanctionedposts_fact" alias="sanctioned_posts">
</Table>
```
