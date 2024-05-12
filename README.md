# **Practice-tasks-1:**

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

```jsx
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
##Noob
## **Find:**

> filtering data releted all datas show.
> 

## **FindOne:**

> ****Only one data filtering
> 

## **Field Filtering:**

```jsx
db.test.find({"gender" : "Female"},{name:1}) //gender Female releted filter but only name value are show
db.test.find({"gender" : "Female"},).project({name:1})
```

## **Operator:**

**$Eq, $Neq, $Gt, $Lt, $Gte, $Lte**

### **$Eq:**

```jsx

db.test.find({gender:{$eq:"Male" }})
```

### $ne:

Not filtering data of  gender:”male” 

```json
db.test.find({gender:{$ne:"Male" }})
```

### $gt:

only show >18 age of datas

```jsx
db.test.find({age: {$gt:18 }})
```

### $gte:

only show ≥18 age of datas.also sorting accending order

```jsx
db.test.find({age: {$gte:18 }}).sort({ age:1 })
```

### $gt and $lt:

when we filter any range of number 

```jsx
db.test.find({age:{$gt:18,$lt: 30}},{age:1}).sort({ age:-1 })
//many datas filter
db.test.find({ gender:"Female",age:{$gt:18,$lt: 30} },{age:1,gender:1}).sort({ age:-1 })
```

### $in:

```
 db.test.find({
gender: "Female",
age: { $nin: [18, 20, 22, 26] },
interests:{$in: ["Cooking","Gamming"]}

},
 { age: 1, gender: 1,interests:1 }).sort({ age: -1 })

```

### **Explicit $And:**

```
db.test.find({$and: [
    {age: {$ne:17}},
    {age: {$lte:30}},
    
    
    ]},
    {age:1}).sort({ age:1 })
```

### **Explicit $or: same work of** $in:

```jsx
db.test.find({$or:[
{"[skills.name](http://skills.name/)":"JAVASCRIPT"},
{"[skills.name](http://skills.name/)":"python"}
]}).project({"skills":1}).sort({skills:1})
//$in used
db.test.find({"interests":{$in: ["Cooking","Gamming"]}}).project({"interests":1}).sort({skills:1})
```

### **$Exists:**

```jsx
db.test.find({skills:{$exists: true}}).sort({skills:1})
```

### **$Type:**

```jsx
db.test.find({age:{$type: "number"}}).sort({skills:1})
//null
db.test.find({company:{$type: "null"}}).project({company:1})
//array of size
db.test.find({friends:{$size:  4}}).project({friends:1})
```

### Position of array:

```jsx
db.test.find({"interests.2":"Cooking"}).project({interests:1})
//output
//"interests" : [ "Gardening", "Gaming", "Cooking" ]
//only show target 3 item of any index position
db.test.find({interests: {$all:[ "Travelling", "Reading", "Cooking" ]}}).project({interests:1})
```

### **$ElemMatch:**

```jsx

db.test.find({
    skills:{$elemMatch: {
        name:"JAVASCRIPT",
        level:"Intermidiate"
        
    }}
}).project({"skills.name":1,"skills.level":1})
```

### update Filed:**$Set**

```jsx
//
db.test.updateOne(
{which property update },
{updated value}
)
//
db.test.updateOne(
    { _id: ObjectId("6406ad63fc13ae5a40000067") },
    {
        $set: {
            age:35
        }
    }
)
```

### **$AddToSet: update likes array**

```jsx
db.test.updateOne(
{ _id: ObjectId("6406ad65fc13ae5a400000c7") },
{
$addToSet: {
interests : [ "Gamingg", ]
}
}
)
```

### $each:Push the value of array

```jsx
//duplicate value not push in array
db.test.updateOne(
{ _id: ObjectId("6406ad65fc13ae5a400000c7") },
{
$addToSet: {
interests : {$each: [ "Gaminggg",  ]}
}
}
)
```

### $push:duplicate value also push the array

```jsx
db.test.updateOne(
{ _id: ObjectId("6406ad65fc13ae5a400000c7") },
{
$push: {
interests : {$each: [ "Gaminggg",  ]}
}
}
)
```

### $unset:any fied remove

```jsx
db.test.updateOne(
{	_id : ObjectId("6406ad65fc13ae5a400000c7")},
{$unset: { birthday:"" } }

)

```

### **$Pop:delete last element of target field**

```jsx
db.test.updateOne(
{	_id : ObjectId("6406ad65fc13ae5a400000c7")},
{$pop: { friends:1} }

)

```

### $pull:

```jsx
db.test.updateOne(
{	_id : ObjectId("6406ad65fc13ae5a400000c7")},
{$pull: { friends:"Najmus Sakib"} }

)

```

### **$PullAll:**

```jsx
db.test.updateOne(
{	_id : ObjectId("6406ad65fc13ae5a400000c7")},
{$pullAll: { friends:[ "Fahim Ahammed Firoz", "Rasel Ahmed" ]} }

)
opkewf0-iewflkdsflfo
```

### **Delete Documents:**

```jsx
//delete one documents.
db.test.deleteOne({_id:ObjectId("6406ad65fc13ae5a400000c7")})
```



## Intro The Powerful Aggregation Framework**

### **1.$Match , $Project Aggregation Stage(First stage)**

when aggeration used then impliments **$match** method

```jsx
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

```jsx
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

```jsx
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

filtering datas with field in add to new collection

```jsx

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

```jsx
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

```jsx
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

full document want to used $push:”$$ROOT”}

```jsx
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

### $Group indetails:

```jsx
db.datas.aggregate([
    //stage-01
   {
       $group: { _id: null,
       Totalsalary:{$sum: "$salary"},
       maxsalary:{$max: "$salary"},
       minsalary:{$min: "$salary"},
       avgsalary:{$avg: "$salary"},
       
       }
           
           
           
       },
 
   //stage-02
   {$project: {Totalsalary:1,
       maxsalary:1,
       minsalary:1,
       avarageSalary:"$avgsalary",
       rangebetweenMaxAndMin:{$subtract: ["$maxsalary","$minsalary"]}
       
   }}
])
```

### **$Unwind Aggregation:**

```jsx
   db.datas.aggregate([
//stage-01
{$unwind: "$friends"},
{
$group: { _id: "$friends",count:{$sum: 1},
    }
])

//another
db.datas.aggregate([
    //stage-01
    { $unwind: "$interests" },
    //stage-02
    { $group: { _id: "$age", interestPerAge: { $push: "$interests" } }}
])
```

### **$Bucket:**

```jsx
db.datas.aggregate([
//stage-01
{
$bucket: {
groupBy: "$age",
boundaries: [ 20, 40, 80 ],
default: "Other",
output: {
"count": { $sum: 1 },
//whichperson array push only name field.full documents push then used $push:”$$ROOT”}
whichPerson:{$push: "$name"}
          }
        }
}
])
```

```jsx
         db.datas.aggregate([
//stage-01
{
$bucket: {
groupBy: "$age",
boundaries: [ 20, 40, 80 ],
default: "Other",
output: {
"count": { $sum: 1 },
whichPerson:{$push: "$name"}
          }
        }
},
//stage-02
{
    $sort: {count:1}
},
{
    // $limit: 2
},
//stage-03
{
    $project: {count:1}
}
])
```

### **$Facet,  Multiple Pipeline Aggregation Stage**

```jsx
db.datas.aggregate([

    {
        $facet: {
            //pipeline-01
            "friendsCount": [
                //stage-01
                { $unwind: "$friends" },
                //stage-02
                { $group: { _id: "$friends", count: { $sum: 1 } } }

            ],
            //pipeline-02
            "educationCount": [
                //stage-01
                { $unwind: "$education" },
                //stage-02
                { $group: { _id: "$education.major", count: { $sum: 1 } } }

            ]
        }
    }

])
```

### **$Lookup Stage:Embedding Vs Referencing**

```jsx
db.orders.aggregate([
    {
        $lookup: {
            from: "datas",
            localField: "userId",
            foreignField: "_id",
            as: "foreignFieldData"
        }
    }

])

```

### **COLLSCAN Vs IXSCAN:**

indexing is the better then collscan.

```jsx
db.datas.find({
    	"_id" : ObjectId("6406ad65fc13ae5a400000c7")
}).explain("executionStats")
   
```
