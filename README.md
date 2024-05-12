> 1.Find all documents in the collection where the age is greater than 30, and
only return the name and email fields.
> 

```jsx
db.test.find({age:{$gt: 30}}).project({name:1,email:1})
```

> 2. Find documents where the favorite color is either "Maroon" or "Blue.”
> 

```jsx
db.test.find(
{favoutiteColor:{$in:["Maroon","Blue"]}}
).project({favoutiteColor:1})
```

> 3. Find all documents where the skill is an empty array.
> 

```json
db.test.find(
{skills:{$type: "null"}}
).project({skills:1})

```

> 4.Find documents where the person has skills in both "JavaScript" and
"Java."
> 

```jsx
 db.test.find({
$and: [
        { "skills.name": "JAVASCRIPT" },
        { "skills.name": "PYTHON" }
    ]
}

).project({ skills: 1 })

```

> 5.Add a new skill to the skills array for the document with the email
"[aminextleveldeveloper@gmail.com](mailto:aminextleveldeveloper@gmail.com)". The skill is
{"name": "Python"
,
"level": "Beginner"
,
"isLearning": true}
> 

```jsx
db.test.updateOne(
{ "email": "[cthame2q@tumblr.com](mailto:cthame2q@tumblr.com)" },
    {
        $push: {
            "skills": {
                "name": "Python",
                "level": "Beginnerrrr",
                "isLearning": true
            }

        }
    }

)

```

> 6.Add a new language "Spanish" to the list of languages spoken by the
person.
> 

```jsx
 db.test.updateOne(
{ "email": "[cthame2q@tumblr.com](mailto:cthame2q@tumblr.com)" },
    {
        $addToSet: {
           languages:"Spanish"

        }
    }

)

```

> 7. Remove the skill with the name "Kotlin" from the skills array.
> 

```jsx
db.test.deleteOne
(
{ "email": "[cthame2q@tumblr.com](mailto:cthame2q@tumblr.com)" },
{$pull: {
        "skills.name":"KOTLIN"
    }}

)

```

# **Practice-tasks-2:**

> 
> 
> 1. Retrieve the count of individuals who are active (isActive: true) for each
> gender.

```jsx
db.massive.aggregate([

    { $match: { isActive: true } },
    {
        $group: { _id: "$gender", count: { $sum: 1 } }
    }

])

```

> 2.Retrieve the names and email addresses of individuals who are active
(`isActive: true`) and have a favorite fruit of "banana".
> 

```jsx
db.massive.aggregate([

    { $match: { isActive: true, favoriteFruit: "banana" } },

    { $project: { favoriteFruit: 1, isActive: 1 } }

])

```

> **3. Find the average age of individuals for each favorite fruit, then sort the results in descending order of average age.**
> 

```jsx
db.massive.aggregate([

    {
        $group: {
            _id: "$favoriteFruit",
            avarageAge: { $avg: "$age" }
        }
    }

]).sort({ avarageAge: -1 })

```

> **4. Retrieve a list of unique friend names for individuals who have at least one friend, and include only the friends with names starting with the letter "W".**
> 

```jsx
db.massive.aggregate([
    { $unwind: "$friends" },
    {
        $match: {
            "friends.name": /^W/
        }
    },
    {
        $group: {
            _id: "$_id",
            uniqueFriends: { $addToSet: "$friends.name" }
        }
    }
]);
```

> **5. Use $facet to separate individuals into two facets based on their age: those below 30 and those above 30. Then, within each facet, bucket the individuals into age ranges and sort them by name within each bucket.**
> 

```jsx
db.massive.aggregate([
  {
    $facet: {
      below30: [
        { $match: { age: { $lt: 30 } } },
        {
          $bucket: {
            groupBy: "$age",
            boundaries: [20, 25, 30],
            default: "Other",
            output: {
              names: { $push: "$name" }
            }
          }
        }
      ],
      above30: [
        { $match: { age: { $gte: 30 } } },
        {
          $bucket: {
            groupBy: "$age",
            boundaries: [30, 35, 40],
            default: "Other",
            output: {
              names: { $push: "$name" }
            }
          }
        }
      ]
    }
  }
]);
```

**6.Calculate the total balance of individuals for each company and display the company name along with the total balance. Limit the result to show only the top two companies with the highest total balance.**

```jsx
db.massive.aggregate([
  {
    $group: {
      _id: "$company",
      totalBalance: { $sum: { $toDouble: { $trim: { input: "$balance", chars: "$," } } } }
    }
  },
  {
    $sort: { totalBalance: -1 }
  },
  {
    $limit: 2
  }
]);
```
