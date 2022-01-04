# ts-polymorphism-over-conditionals

# Javascript/ Typescript 
## How to avoid using more conditionals then we absolutely have to,

### A Solution for your Switch Case Hell!

Most of the times we find ourselves using a lot of conditionals in our code. It seems impossible to avoid the use of if statements inside our codebase. Since they are the building blocks of any programming language it is almost second nature to go for if statements and if the conditions are long enough the dreaded switch case.

As people debate on the efficiency of if statements over switch case, I am here to say you need to stop using them altogether.(well mostly anyway.)

Let try some conditional coding.


```
class ConditionalOrders {

    constructor(details:string){}

    order(type:string){

        switch(type){

            case 'Nepali':
                // Run some code here
                break;

            case 'Indian':
                //Run some code here
                break;

        }
    }

    bill(type:string){
        
        switch(type){

            case 'Nepali':
                // Run some code here
                break;

            case 'Indian':
                //Run some code here
                break;

        }
    }

}  
```

If the problems with this type of pattern  isn't immediately apparent, let me point them out.

* Imagine you had to add another Order type for your system. You would have to go through all the switch cases in existence and add another condition to all of them. Conveniently this example only has two methods, theoretically your system could have many more.
* We are also clearly violating one of the key principles of clean code when we add another type into our system. The open close principle aka your code much be open for extension and closed for change.
* The maintainability of the code takes a massive hit with each new case added.

These are only few and I could probably google way more.

# Now lets Get to Our Solution for this conditional Mess.

## Polymorphism.

Yes polymorphism can single handedly eliminate the use of most of our switch cases.
Lets convert the above code by using polymorphism.

### Step 1 : Convert the Order class into an Abstract class.


```
abstract  class  Orders {
	constructor(public details:string){}
	abstract  order():void;
	abstract  bill():void;

}
```

### Step 2: Create Seperate classes for Each type of Order and extend the abstract class.

```
class  NepaliOrder  extends  Orders{
constructor(details:string){
super(details);
}
order():  void {
// Run some code here
}
bill():  void {
// Run some code here
}
}

  

class  IndianOrder  extends  Orders{
constructor(details:string){
super(details);
}
order():  void {
// Run some code here
}

bill():  void {
// Run some code here
}
}
```

### Step 3 Now to instantiate the class dynamically 

Create an object that maps the class to the type of order.

```
const  ClassCollection: {[key:string]:typeof NepaliOrder} = {
	'Nepali': NepaliOrder,
	'Indian':IndianOrder
}
```
Now lets mock a request from a user 

```
const  request  = {
	details:'6790234',
	type:'Nepali'
}

  
  

const  requestObject  =  new ClassCollection[request.type](request.details)
requestObject.order();
requestObject.bill();
```
