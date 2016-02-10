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
here `name` is our table name from mysql database and alias is just an alternative name which can be refered to if there are other tables refered from this cube (in our case it's not needed)

Now we need some measures/values to see and some Hierarchial dimention (with some Levels) to see them against.

Let's first define the Dimensions.

Say we want to see some values against Hierarchial Area (Division -> District -> Upozilla). So we need to define the Dimention/Hierarchy/Level like this

```xml

<Dimension type="StandardDimension" visible="true" highCardinality="false" name="Facility">
  <Hierarchy name="Area" visible="true" hasAll="true" allMemberName="All Area" defaultMember="All Area">
    <Level name="Division" visible="true" column="division_id" nameColumn="division_name" type="String" uniqueMembers="true" levelType="Regular" hideMemberIf="Never" caption="Division">
    </Level>
    <Level name="District" visible="true" column="district_id" nameColumn="district_name" type="String" uniqueMembers="true" levelType="Regular" hideMemberIf="Never" caption="District">
    </Level>
    <Level name="Upozilla" visible="true" column="upazila_id" nameColumn="upazila_name" type="String" uniqueMembers="true" levelType="Regular" hideMemberIf="Never" caption="Upozilla">
    </Level>
  </Hierarchy>
</Dimension>

```

Similarly for Designation we can do this

```xml

<Dimension type="StandardDimension" visible="true" highCardinality="false" name="Designation">
  <Hierarchy name="Designation" visible="true" hasAll="true" allMemberName="All Designation" defaultMember="All Designation">
    <Level name="Designation Class" visible="true" column="designationclass_id" nameColumn="designationclass_name" type="String" uniqueMembers="true" levelType="Regular" hideMemberIf="Never" caption="Class">
    </Level>
    <Level name="Designation" visible="true" column="designation_id" nameColumn="designations_name" type="String" uniqueMembers="false" levelType="Regular" hideMemberIf="Never" caption="Designation">
    </Level>
  </Hierarchy>
</Dimension>

```

Time Dimention should be mentioned as a different 'type' in mondrian schema. Example:

```xml

<Dimension type="TimeDimension" visible="true" highCardinality="false" name="Time">
  <Hierarchy visible="true" hasAll="true" allMemberName="All Year" defaultMember="All Year">
    <Level name="Year" visible="true" column="s_year" type="String" uniqueMembers="true" levelType="TimeYears" hideMemberIf="Never" caption="Year">
    </Level>
    <Level name="Month" visible="true" column="s_month" nameColumn="s_monthname" type="String" uniqueMembers="true" levelType="TimeMonths" hideMemberIf="Never" caption="Month">
    </Level>
  </Hierarchy>
</Dimension>

```

All these dimention tag can go inside or outside the Cube tag. If it's outside the Cube tag, we need to define DimentionUsage tag for refering to the Dimension. In our case, all Dimentions are inside the Cube.

Also dimention can be linked to other Table(s), in that case we need another Table tag and foreign_key primary_key relation. We don't need that yet

Finally let's define some Measures inside the Cube

```xml

<Measure name="Sanctioned Posts" column="id" aggregator="count" visible="true">
</Measure>
<Measure name="Filled" column="Filled" aggregator="sum" visible="true">
</Measure>
<Measure name="Vacant" column="Vacant" aggregator="sum" visible="true">
</Measure>
<Measure name="Male" column="Male" aggregator="sum" visible="true">
</Measure>
<Measure name="Female" column="Female" aggregator="sum" visible="true">
</Measure>

```

The final schema can be found in this repository under the name `hrm-mondrian.xml`

That's all.
