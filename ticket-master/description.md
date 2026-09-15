# Ticket Master

Problem Statement
Design a system that sells tickets to events with assigned seats: users browse what's available, pick a seat, and buy it. The same seat must never sell twice.

In scope:

- browsing seat availability
- holding a seat during checkout
- purchasing a held seat
- releasing an expired or abandoned hold.

Out of scope:

- dynamic pricing
- seat recommendations
- the internals of the payment provider itself

## Clarifying Questions (Ask the interviewer)

- Assigned seats or general admission?
  - Assigned seats — the hard case, where each seat is a unique unit of inventory. General admission, a single counter per tier, is a variant.

- Assigned seats or general admission?
  - Assigned seats — the hard case, where each seat is a unique unit of inventory. General admission, a single counter per tier, is a variant.

- Is overselling ever acceptable?
  - No. Selling one seat twice is a correctness failure, so inventory is strongly consistent even at the cost of availability.

- How long does a hold last?
  - A few minutes — long enough to check out, short enough that an abandoned cart returns to inventory quickly.

- Are payments in scope?
  - No. Checkout hands off to an existing payment system; this design focuses on inventory and the on-sale stampede.

## What is the hard/interest part of the problem

The difficulty isn't browsing. It's that a popular on-sale is a synchronized stampede — on the order of a hundred thousand people trying to grab the same few thousand seats in the same few seconds — and inventory correctness must hold exactly under that contention.

> Key idea: A popular on-sale is a synchronized stampede against a small, fixed inventory — the design needs exactly one winner per seat and a way to survive the spike that produces that winner.

## Key concepts

- The hold
  - A hold is a short-lived, temporary reservation on a seat: it locks the seat for one buyer while they check out, and automatically expires if they don't complete the purchase in time.
  - A hold is what separates "reserved" from "sold," and its expiry is what keeps an abandoned cart from stranding a seat forever.

- Atomic compare-and-set
  - A seat's state transition from available to held has to happen as a single, indivisible operation: update the row only if its available
  - When thousands of requests request for the same seat, the DB should searilize the request and only the first request should see the seat as aviable and mark it as reserved
  - The other seats should see the seat status as reserved (and should fail)
  - This is why the transition should be single atomic ops, not a read followed by a separate write, is what prevents overselling.
  - > Compare-and-set (CAS). A conditional write: "set X to value B, but only if X currently equals A." If another writer changed X first, the CAS fails cleanly instead of overwriting a change it never saw.

- The waiting room
  - A virtual queue issues each arriving user a token and admits them into the buying flow at a rate the inventory service can actually handle, turning a simultaneous wall of demand into a controlled stream
  - Admission into the waiting room is not a guarantee of a seat; it only guarantees the inventory service gets to process requests at a survivable rate.
- Cached browse, consistent hold
  - For the seat browsing requests, the data can come from an eventually consistent cache.
    - In the worst case with stale data, if the seat is taken/holded, but not updated (because stale data), the user proceeds with purchasing it, but CAS catches and throws error
  - Hold Seat service has to be strongly consistent
  - Read/browse traffic which is much larger than seat hold requests/traffics never touch/overlap with each other and should have different consistent models

## Requirements

Functional Requirement

- browse the event seats
- hold the seat and purchase it
- Release the expire holds back to available

Non-Functional Requirement

- Strong consistency for ticket booking (no 2 users get the same seat/ticket)
- High availability for seat browsing
- low latency for ticket booking
- handle high traffic for popular events
