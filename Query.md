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

```json
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

```jsx
 db.test.find({
gender: "Female",
age: { $nin: [18, 20, 22, 26] },
interests:{$in: ["Cooking","Gamming"]}

},
 { age: 1, gender: 1,interests:1 }).sort({ age: -1 })

```

### **Explicit $And:**

```jsx
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

```json
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
