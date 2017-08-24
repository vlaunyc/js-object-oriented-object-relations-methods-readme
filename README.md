# Associating Objects in Javascript

## Objectives
+ Learn how to build methods to select associated objects.
+ Learn how to build methods to find objects by certain attributes.

### Associating Objects
In the previous lesson we saw how to associate objects through use of a store and adding functionality to our JavaScript classes.  Let's take another look at that code.

```javascript
let store = {rooms: [], hotels: []}
// initialize store with key of rooms and hotels that each point to an empty array

let hotelId = 0

class Hotel {
  constructor(name){
    this.id = ++hotelId
    this.name = name

    // insert in the hotel to the store
    store.hotels.push(this)
  }
}

let roomId = 0

class Room {
  constructor(roomNumber, squareFeet, hotel){
    this.id = ++roomId
    this.squareFeet = squareFeet
    this.roomNumber = roomNumber
    if(hotel){
      this.hotelId = hotel.id
    }

    // insert in the room to the store
    store.rooms.push(this)
  }
  setHotel(hotel){
    this.hotelId = hotel.id
  }
}

let forest = new Hotel("The Forest Hotel")
let forestRoom = new Room(213, 240, forest)
let penthouseForestRoom = new Room(1200, 500, forest)

store
// {hotels: [{id: 1, name: "The Forest Hotel"}], rooms: [{id: 1, number: 213, squareFeet: 240, hotelId: 1},
// {id: 2, number: 1200, squareFeet: 500, hotelId: 1}
// ]}
```

Now the next thing we need to do is build some methods that would select the associated data.  For example, if we want to find all of the rooms associated with the first hotel, we would like to write a method called `rooms()` that would retrieve all of the rooms associated with the first hotel:

```js
  let forestHotel = store.hotels[0]  
  forestHotel.rooms()
  // we want this to return our first two rooms in our store, but currently this method is not implemented
```

Ok, now how would we implement a method on our hotel object that finds the associated rooms?  Well, the way we can identify the rooms associated with the first hotel is go to our store, and go through each of the rooms in our store and return the ones with a hotelId equal to 1.  We can use JavaScript's `filter` method to do just that.  Let's go for it!

```javascript
class Hotel {
  constructor(name){
    this.id = ++hotelId
    this.name = name

    // insert in the hotel to the store
    store.hotels.push(this)
  }
  rooms(){
    return store.rooms.filter(function(room){
      return room.hotelId === this.id
    })
  }
}
```

So you can see that the code above uses the filter method to go through the rooms in the store and return each of the rooms that have a hotelId equal to the id of the hotel receiving the rooms method call.  Ok, that was the hard one, now let's write a method such that we can call `room.hotel()` and the hotel associated with the room is returned.

```js

class Room {
  constructor(roomNumber, squareFeet, hotel){
    this.id = ++roomId
    this.squareFeet = squareFeet
    this.roomNumber = roomNumber
    if(hotel){
      this.hotelId = hotel.id
    }

    // insert in the room to the store
    store.rooms.push(this)
  }
  setHotel(hotel){
    this.hotelId = hotel.id
  }
  hotel(){
    return store.hotels.find(function(hotel){
      return hotel.id === this.hotelId
    })
  }
}

let hotel = new Hotel('Nightcrawl')
let room = new Room(242, 250, hotel)
room.hotel()
// {id: 2, name: 'Nightcrawl'}
```

Unlike our use of the `filter` method, JavaScript's `find` method only returns the first matching element from the array.  With our `rooms()` added to our hotel objects and the `hotel()` method added to our room objects  we have set up our relationship in both directions.

## Summary

In this lesson, we saw how to write methods to select our associated data.  We saw that by using JavaScript's `filter` and `find` methods we can search the store to return the proper JavaScript objects when our methods are called.

## Resources

+ [Sql Relations](https://github.com/learn-co-curriculum/sql-table-relations-readme)

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/js-classes-readme'>Classes in JS</a> on Learn.co and start learning to code for free.</p>
