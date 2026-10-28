# Week 3 Practice — Answers

## 1. What goes wrong if you use the array index as `:key` and the list gets filtered while a card has local state?

Using the array index as `:key` can cause Vue to reuse the wrong component when the list is filtered or reordered. For example, an open details panel might appear on a different event because Vue identifies the card by its position instead of its unique event ID. Using `:key="e.id"` helps Vue track each event correctly.

## 2. Why is `visible` a computed and not a function called from the template? What is the difference when you type in the search box?

`visible` is a computed property because its value depends on the search text, selected category, and events array. Vue automatically recalculates it when its dependencies change and caches the result until they change again. A function called from the template may execute again during each render, even when its relevant data has not changed.

## 3. EventCard emits `book` instead of changing `event.booked`. Give two reasons.

First, props are read-only, so `EventCard` should not directly modify `event.booked`. Second, emitting a `book` event keeps the component reusable and separates responsibilities: `EventCard` reports the user's action, while the parent component decides whether to update the booking data and enforce the seat limit.

## 4. You write `const { booked } = reactive(event); booked++`. The page does not change. Why?

Destructuring `booked` from a reactive object creates a separate local variable that is no longer connected to the original object's reactive property. Increasing this variable does not update `event.booked`, so Vue cannot detect the intended change through the original reactive object. Instead, update `event.booked` directly or use `toRef(event, 'booked')` to preserve reactivity.

## 5. Where does `myBookings` live, and why not in EventCard or EventList?

`myBookings` lives in `App.vue` because `App` owns the events and controls the booking updates. `EventCard` only displays one event and emits booking requests, while `EventList` filters and displays events and forwards those requests. Keeping the booking state in the parent ensures that the event data and booking summary stay consistent.