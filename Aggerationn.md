#Intro The Powerful Aggregation Framework

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
