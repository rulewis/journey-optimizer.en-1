---
title: Arrays functions library
description: Arrays functions library
feature: Personalization
topic: Personalization
role: Data Engineer
level: Experienced
exl-id: dfe611fb-9c50-473c-9eb7-b983e1e6f01e
---
# Arrays and list functions {#arrays}

Use these functions to make interaction with arrays, lists, and strings easier.

## Distinct{#distinct}

The `distinct` function is used to get values from an array or list with duplicate values removed.

**Format**

```sql
{%= distinct(array) %}
```

**Example**

The following operation specifies people who have placed orders in more than one store.

```sql
{%= distinct(person.orders.storeId).count() > 1 %}
```

## First item{#head}

The `head` function is used to return the first item in the array or list.

**Format**

```sql
{%= head({array}) %}
```

**Example**

The following operation returns the first of the top five orders with the highest price. More information about the `topN` function can be found in the [first `n` in array](#first-n) section.

```sql
{%= head(topN(orders,price, 5)) %}
```

## First `n` in array {#first-n}

The `topN` function is used to return the first `N` items in an array, when sorted in ascending order based on the given numerical expression.

**Format**

```sql
{%= topN(array, value, amount) %}
```

| Argument | Description |
| --------- | ----------- |
| `{ARRAY}` | The array or list that is to be sorted. |
| `{VALUE}` | The property in which to sort the array or list. |
| `{AMOUNT}` | The number of items to be returned. |

**Example**

The following operation returns the top five orders with the highest price.

```sql
{%= topN(orders,price, 5) %}
```

## In{#in}

The `in` function is used to determine if an item is a member of an array or list.

**Format**

```sql
{%= in(value, array) %}
```

**Example**

The following operation defines people with birthdays in March, June, or September.

```sql
{%= in (person.birthMonth, [3, 6, 9]) %}
```

## Includes{#includes}

The `includes` function is used to determine if an array or list contains a given item.

**Format**

```sql
{%= includes(array,item) %}
```

**Example**

The following operation defines people whose favorite color includes red.

```sql
{%= includes(person.favoriteColors,"red") %}
```

## Intersects{#intersects}

The `intersects` function is used to determine if two arrays or lists have at least one common member.

**Format**

```sql
{%= intersects(array1, array2) %}
```

**Example**

The following operation defines people whose favorite colors include at least one of red, blue, or green.

```sql
{%= intersects(person.favoriteColors,["red", "blue", "green"]) %}
```


<!-- ## Intersection{#intersection}

The `intersection` function is used to determine the common members of two arrays or lists.

**Format**

```sql
intersection({ARRAY},{ARRAY})
```

**Example**

The following operation defines if person 1 and person 2 both have favorite colors of red, blue, and green.

```sql
intersection(person1.favoriteColors,person2.favoriteColors) = ["red", "blue", "green"]
```
--> 

## Last `n` in array{#last-n}

The `bottomN` function is used to return the last `N` items in an array, when sorted in ascending order based on the given numerical expression.

**Format**

```sql
{%= bottomN(array, value, amount) %}
```

| Argument | Description |
| --------- | ----------- | 
| `{ARRAY}` | The array or list that is to be sorted. |
| `{VALUE}` | The property in which to sort the array or list. |
| `{AMOUNT}` | The number of items to be returned. |

**Example**

The following operation returns the top five orders with the lowest price.

```sql
{%= bottomN(orders,price, 5) %}
```


## Not in{#notin}

The `notIn` function is used to determine if an item is not a member of an array or list.

>[!NOTE]
>
>The `notIn` function *also* ensures that neither value is equal to null. Therefore, the results are not an exact negation of the `in` function.

**Format**

```sql
{%= notIn(value, array) %}
```

**Example**

The following operation defines people with birthdays that are not in March, June, or September.

```sql
{%= notIn(person.birthMonth ,[3, 6, 9]) %}
```


## Subset of{#subset}

The `subsetOf` function is used to determine if a specific array (array A) is a subset of another array (array B). In other words, that all elements in array A are elements of array B.

**Format**

```sql
{%= subsetOf(array1, array2) %}
```

**Example**

The following operation defines people who have visited all of their favorite cities.

```sql
{%= subsetOf(person.favoriteCities,person.visitedCities) %}
```

## Superset of{#superset}

The `supersetOf` function is used to determine if a specific array (array A) is a superset of another array (array B). In other words, that array A contains all elements in array B.

**Format**

```sql
{%= supersetOf(array1, array2) %}
```

**Example**

The following operation defines people who have eaten sushi and pizza at least once.

```sql
{%= supersetOf(person.eatenFoods,["sushi", "pizza"] %}
```
