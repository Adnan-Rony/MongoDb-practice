# MongoDb-Practice

> 1.Find all documents in the collection where the age is greater than 30, and
only return the name and email fields.
> 

```json
db.test.find({age:{$gt: 30}}).project({name:1,email:1})
```

> 2. Find documents where the favorite color is either "Maroon" or "Blue.”
> 

```json
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

```json
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

```json
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

```json
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
>db.test.deleteOne
(
{ "email": "cthame2q@tumblr.com" },
{$pull: {
        "skills.name":"KOTLIN"
    }}




)