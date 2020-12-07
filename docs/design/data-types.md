# Meal Planner Data Types

This is a document outlining the different types of data which
will be required for the meal planner, and how they should fit
together.

This roughly corresponds to the SQL schemas we'll end up with, and will
be updated as our data models evolve.

Note that this is supposed to be a simple personal-use tool, 
so we don't care about trading JOINs for efficiency, since this is not operating
on a scale where that matters. We're optimizing for space-efficiency instead.

## Ingredient

Ingredient is a single ingredient which may show up in a recipe.

### Schema

| field | type | description |
| --- | --- | --- |
| name | text | the name of the ingredient |
| description | text | a human-readable description of the ingredient |

## Recipe

Recipe is a recipe which may appear in a meal plan.

The difficulty rating is as follows:

1. very easy
2. easy
3. intermediate
4. hard
5. very hard

_Right now, we can just have this be a user-defined value, but maybe
there can be some kind of rating system where you rate how hard it is
every time you make it, and the final rating is an average of all the given
ratings. (spitballing)_

### Schema

| field | type | description |
| --- | --- | --- |
| name | text | the name of the recipe |
| description | text | a human-readable description of the recipe |
| difficulty | uint | a five point rating for difficulty |

## Unit

A Unit is a unit of measurement that an ingredient may occupy.

_This could easily exist as a text field or something, but we're storing it 
in a table so we can do things like conversion rates later. IE. we can group units into
imperial/metric and do a conversion between them using some conversion factor defined
elsewhere._

### Schema

| field | type | description |
| --- | --- | --- |
| name | text | the name of this unit |
| description | text | a human readable of the significance of this unit |

## RecipeStep

A recipe has a set of steps associated with it which must be performed in order. 
This data type lets us quantize each step in a recipe, and organize/rearrange them.

Some steps can be performed at the same time, so multiple steps can share the 
same "ordinal" number.

A step is also where we define where some quantity of an ingredient is being used.
We can compute the total amount of ingredients needed for all steps in code. A step should always involve 
at most 

### Schema

| field | type | description |
| --- | --- | --- |
| ordinal | uint | the ordinal number of this step |
| description | text | human-readable description of the step |

## RecipeStepIngredientQuantity

A recipe step needs a set of ingredients with the quantities required in said 
step. This data type forms a relationship between a recipe and the amounts 
of ingredients that appear in said recipe.

### Schema

| field | type | description |
| --- | --- | --- |
| recipe_step_id | int | the primary key of the step this quantity is needed for |
| unit_id | int | the primary key of whatever unit this quantity should use |
| quantity | float | the amount of whatever ingredient this is (fractions can be computed back from this for display purposes) |
