---
layout: essay
type: essay
title: "Design Patterns: Building Blocks of Better Code"
# All dates must be YYYY-MM-DD format!
date: 2024-04-25
published: True
labels:
  - Design Patterns
  - Software Engineering
  - React
---

<img width="300px" class="rounded float-start pe-4" src="../img/design-patterns.jpg">

## What are Design Patterns?
Design patterns refer to reusable solutions to common problems that occur during the design and development of software systems. These patterns include best practices, proven strategies, and general guidelines for designing flexible, scalable, and maintainable code. 

Design patterns provide a structured approach to solving specific design challenges, offering developers well-defined templates/blueprints to follow. They promote code reusability, modularization, and abstraction, leading to better software design in general.

## Have I Used Any Design Patterns?
YES! I'm using them right now! 

Currently, I'm working with a team to develop an application displaying food vendors on the UH Manoa campus. We are using a MongoDB collection to store all our vendors and their information.

Singleton Example - Vendor Collection:
```
class VendorsCollection {
  constructor() {
    this.name = 'VendorsCollection';

    this.collection = new Mongo.Collection(this.name);

    this.schema = new SimpleSchema({
      name: String,
      image: String,
      location: {
        type: String,
        allowedValues: ['Campus Center', 'Paradise Palms', 'Food Truck Row', 'Hemenway Hall', 'Residential Dining'],
      },
      hours: String,
      owner: String,
      menuImage: String,
    });

    this.collection.attachSchema(this.schema);
    this.userPublicationName = `${this.name}.publication.user`;
    this.vendorPublicationName = `${this.name}.publication.vendor`;
    this.adminPublicationName = `${this.name}.publication.admin`;
  }
}

export const Vendors = new VendorsCollection();
```
This 'VendorsCollection' class is designed to be a singleton instance, ensuring that only one instance of the 'VendorsCollection' is created and used throughout the application. The variable 'Vendors' is the singleton instance that we export and use when we want to manipulate the collection. 

Whenever we want to display information from the Vendors collection, whenever we want to add, edit, or remove stuff in the collection, we use that 'Vendors' variable which is used all throughout our application.

## Reactivity Example - Meteor Subscriptions:
```
Meteor.publish(Vendors.userPublicationName, function () {
  return Vendors.collection.find({});
});
```
```
const CampusCenter = () => {
  const { ready, vendor } = useTracker(() => {
    const subscription = Meteor.subscribe(Vendors.userPublicationName);
    const rdy = subscription.ready();
    const vendorItems = Vendors.collection.find({}).fetch();
    return {
      vendor: vendorItems,
      ready: rdy,
    };
```

Inside the useTracker hook, the code establishes a reactive connection between the subscribed data and the React component 'CampusCenter'. This means that whenever the subscribed data changes, the component will be automatically re-rendered with the updated data.

## Conclusion
These are just a few examples of design patterns that I'm currently using and doesn't even begin to cover all the other design patterns being used on this project, or even design patterns I have used on other projects in the past. Design patterns are essential tools for software developers that offer reusable solutions to common design challenges. 
