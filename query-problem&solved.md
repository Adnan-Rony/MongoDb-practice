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
# Conceptual Session problem (Query)

> 1.Find all users who are located in `New York`.
> 

```jsx
db.peoples.find({
    "address.city":"Warsaw"
})

```

> 2.Find the user(s) with the email "[johndoe@example.com](mailto:johndoe@example.com)" and retrieve their favorite movie.
> 

```jsx
db.users.find({

    "email": "johndoe@example.com"
}).project({ "favorites.movie": 1, email: 1 })

```

> 3.Delete the user with the email `"alicewilliams@example.com"` from the user data.
> 

```jsx
db.users.deleteOne({

    "email": "johndoe@example.com"
})

```

> 4.Find the user(s) with the highest age.
> 

```jsx
db.users.find().sort({ age:-1 }).project({age:1})

```

> 5.Count all the people named `Pauline Fournier`.
> 

```jsx
db.peoples.find({
    "firstName": "Pauline", "lastName": "Fournier"
}).count()

```

> 6.Count all people whose name is `Pauline Fournier` and who were born before `January 1, 1970`.
> 

```jsx
db.peoples.find({
    "firstName": "Pauline", "lastName": "Fournier", birthDate: { $lt: ISODate("1970-01-01") }
}).count()

```

> 7.Count all people with names:
> 
> - `Lucas Dubois`
> - `Camille Dubois`

```jsx
db.peoples.find({
    $or: [{ "firstName": "Lucas ", "lastName": " Dubois" }, { "firstName": "Camille", "lastName": "Dubois" }]
}).count()

```

> 8.Count all the people who have `no credits`.
> 
> 
> You can find credits in the field `wealth.credits`., this field is an array, because people can have one or more credits, if the array is empty then there are no credits
> 

```jsx
db.peoples.find({
    "wealth.credits": { $size: 0 }
}).count()

```

> 9.Count everyone who spent exactly `$12.99` on the `cinema`.
> 
> 
> All payments are stored in the payments array field, let's take a look at the structure of the elements in this array to write a reasonable query
> 

```jsx
db.peoples.find({
    "payments": { $elemMatch: { "amount": 12.99, "name": "cinema" } }
}).count()

```

> 10.Count all the people whose first payment was `$12.99` for the `cinema`.
> 
> 
> In this lesson, you only count cases where `payments[0]` satisfy the above requirement.
> 

```jsx
db.peoples.find({
    "payments.0.amount": 12.99, "payments.0.name": "cinema"
}).count()

```

> 11.Count all the people who have `never been to the cinema` (no cinema payments yet).
> 

```jsx
db.peoples.find({
    "payments": { $not: { $elemMatch: { "name": "cinema" } } }
}).count()

```

> 12.Count all the `female` who spent more than `$100` on `shoes` and more than `$50` on `pants` in one bill.
> 

```jsx
db.peoples.find({
    $and: [
        { "sex": "female" },
        { "payments": { $elemMatch: { "name": "shoes", "amount": { $gt: 100 } } } },
        { "payments": { $elemMatch: { "name": "pants", "amount": { $gt: 50 } } } }
    ]
}).count()

```

> 13.Count all the people from `Warsaw`, `Poland` who have been to the `cinema` but never to the `disco`.
> 

```jsx
db.peoples.find({
    $and: [
        { "address.city": "Warsaw" },
        { "address.country": "Poland" },
        { "payments": { $elemMatch: { "name": "cinema" } } },
        { "payments": { $not: { $elemMatch: { "name": "disco" } } } }
    ]
}).count()

```

> 15.Count all the people who have exactly `10` transactions
> 

```jsx
db.peoples.find({
    "payments": { $size: 10 }
}).count()

```

> 16.Finds all people with firstName = `'Thomas'` and returns only the following fields: `_id`, `firstName` and `lastName`.
> 

```jsx
db.peoples.find({"firstName": "Thomas"}, {"firstName": 1, "lastName": 1}).pretty()
```

> 17.Find everyone who has one or more transactions worth less than `$5`.
> 
> 
> The returned results only include the `firstName`, `lastName` and `payments` fields containing only the first word whose amount is less than 5$.
> 

```jsx
db.peoples.find({"payments": {$elemMatch: {"amount": {$lt: 5}}}}, {"firstName": 1, "lastName": 1, "payments.$": 1}).pretty()
```

> 18.Add an element to the payments of people who are in `France` with the following structure:
> 
> 
> `{
>  category: "relax",
>  name: "disco",
>  amount: 5.06
> }`
> 

```jsx
db.peoples.updateMany({ "address.country": "France" },
    {
        $addToSet: {
            "payments":
            {
                "category": "relax",
                "name": "disco",
                "amount": 5.06
            }
        }
    })
```

> 19.Delete all of everyone's `market` fields.
> 

```jsx
db.peoples.updateMany({},{$unset: {"wealth.market" : ""}})
```
