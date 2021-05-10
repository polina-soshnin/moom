## Objects as processes

Alan Kay: “local retention and protection and hiding of state-process”

- bundling up state and process together

## Process Objects

- These are not service objects
- "transactional fallacy"
- Shipping monkey example: state change is async, we call out to a service and wait for a callback.
- 2 main bits of behavior, initiate purchase and complete purchase. Both take same amount of data.
- Behavior
- Identity
- State
- These three properties give us an object.
- Treat the state change as an event instead of a command.
- Fallacy: seeing a monkey purchasing series of events as a Transaction, a discrete event.
- Transactions work well with service objects.
- When you have 2 related transactions you now have a process.
- Anything that is 1 discrete action is a transaction.
- Anything that is a series of transactions is a process (user signups, purchases).
- Banking transactions aren't transactional, they are a process and take time to settle.

Old

```
result = Purchase.perform
if result 
"success"
end
```

New

```
purchase = Purchase.new
purchase.submitted # 
if purchase.state == complete?
  "success"
end
```
I would prefer it to be this way:

```
purchase = Purchase.new
purchase.submit!
if purchase.completed?
  "success"
end
```

Food for thought:

```
Food for thought: The exercise is simple: pick five “commanding” method names from your current codebase. They should methods from model or service objects, and they should be methods which, when successful, result in a state change or some interaction with the outside world.

For each one, can you identify and name the event which prompts the action? Instead of telling your objects what to do, what’s the occurrence you could be notifying them of?
```

## Notify don't tell

- Naming the method for an event instead of a command is weird.
- Purchase.shipping_approved instead of "purchase.process_payment"
- Sending event notifications instead of commands gives the controller "humility"

## Process Objects everywhere

- three external events that feed into our process
- Purchase Submitted, Shipping Approved, Payment Cleared.
- Very weird that initiating payment happens in the Shipping Approved method.

Processes:
- Bank Transfer: initiated, settled
- Purchase: ready, pending shipping approval, pending payment, complete
- User: invited, accepted, verified

Static Entities:
- Loan Application: I do not agree with this one as a static entity
- User Account
- Product
- "I am a stable process"

Next time you have a tough object modeling exercise, ask yourself:
- Can I solve this be identifying an implicit process and embodying itself as an object?
- 


