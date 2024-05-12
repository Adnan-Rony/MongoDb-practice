# MongoDb-Practice

> 1.Find all documents in the collection where the age is greater than 30, and
> only return the name and email fields.

```json
db.test.find({age:{$gt: 30}}).project({name:1,email:1})
```

> 2. Find documents where the favorite color is either "Maroon" or "Blue.”

```json
db.test.find(
{favoutiteColor:{$in:["Maroon","Blue"]}}
).project({favoutiteColor:1})
```

> 3. Find all documents where the skill is an empty array.

```json
db.test.find(
{skills:{$type: "null"}}
).project({skills:1})

```

> 4.Find documents where the person has skills in both "JavaScript" and
> "Java."

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
> "[aminextleveldeveloper@gmail.com](mailto:aminextleveldeveloper@gmail.com)". The skill is
> {"name": "Python"
> ,
> "level": "Beginner"
> ,
> "isLearning": true}

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
> person.

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
>    db.test.deleteOne
>    (
>    { "email": "cthame2q@tumblr.com" },
>    {$pull: {

        "skills.name":"KOTLIN"
    }}

)

#Module-02
## 

### **1.$Match , $Project Aggregation Stage(First stage)**

when aggeration used then impliments **$match** method

```json
//aggregate method
db.datas.aggregate([
// stage-01
{$match: {gender:"Male",age:{$lt:30}}}
])

//normal method
db.datas.find({
    
    gender:"Male",age:{$lt:30}
})
 
```

### **2.$Match , $Project Aggregation Stage(second stage)**

```json
db.datas.aggregate([
// stage-01
{$match: {gender:"Male",age:{$lt:30}}},
//stage-02
{$project: {name:1,gender:1,age:1}}
])

//normal method
db.datas.find({

    gender: "Male", age: { $lt: 30 }
}).project({
    name: 1,
    gender: 1,
    age: 1
})

```

### **$AddFields :**

addFields not effect to orginal data,only add to pipeline datas.

```json
db.datas.aggregate([
// stage-01
{ $match: { gender: "Male", } },
//stage-02
{$addFields: {course:"level-02"}},
//stage-03
{$project: {gender:1,course:1}}
])
```

### $out:

filtering datas with field add to new collection

```json

db.datas.aggregate([
// stage-01
{ $match: { gender: "Male", } },
//stage-02
{$addFields: {course:"level-02"}},
//stage-03
{$project: {gender:1,course:1}},
//stage-04
{$out: "course-students"}
])
```

### **$Merge Aggregation:**

new fields data add to orginal collection not another collection;

```json
db.datas.aggregate([
// stage-01
{ $match: { gender: "Male", } },
//stage-02
{$addFields: {course:"level-02"}},
//stage-03
// {$project: {gender:1,course:1}},
//stage-04

{$merge: "datas"}
])
```

### **$Group:**

```json
db.datas.aggregate([
//stage-01
{$group: { _id: "$gender"}}
])
//also add count
db.datas.aggregate([
    //stage-01
    { $group: { _id: "$gender", count: { $sum: 1 } } }
])
```

## **$Push Aggregation Stage:**

```json
 db.datas.aggregate([
//stage-01
{ $group: { _id: "$address.country", fulldocts: { $push: "$$ROOT" } } },
//stage-02
{
$project: {"fulldocts.name.firstName":1,
"fulldocts.email":1
}
    }
])
```

