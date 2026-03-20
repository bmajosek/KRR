# 1. CORE IDEA (WHAT THIS PROJECT IS)

Your entire project is about:

👉 modeling a world that changes over time  
👉 due to actions performed by agents  

At its core, this is a:

## STATE TRANSITION SYSTEM

$$
S_t \rightarrow S_{t+1}
$$

So every step:
- you have a current state
- actions happen
- you compute the next state

---

### Mental model

Time 0: door closed, light off  
Time 1: someone opens the door  
Time 2: someone turns on the light  

Your system:
- stores this
- simulates it
- answers questions about it

---

# 2. FLUENTS ($\mathcal{F}$)

$$
\mathcal{F} = \{f_1, f_2, \dots, f_n\}
$$

Fluents are:

👉 properties of the world

Examples:
- doorOpen
- lightOn
- windowClosed

---

### Intuition

A fluent = a boolean variable  

In Python:
👉 just a string

---

# 3. STATE ($S_t$)

$$
S_t \subseteq Lit(\mathcal{F})
$$

A state is:

👉 a set of facts that are TRUE

Example:

$$
S_t = \{doorOpen, lightOn\}
$$

---

### Important idea

We usually store ONLY TRUE facts.

So:
- if something is in the set → true
- if not → false

---

### Consistency

$$
\neg \big( (f \in S_t) \land (\neg f \in S_t) \big)
$$

Meaning:
👉 you cannot have both:  
doorOpen AND ¬doorOpen

---

### Completeness

$$
(f \in S_t) \lor (\neg f \in S_t)
$$

Meaning:
👉 every fact must have a value

---

# 4. INTERPRETATION & MODEL

$$
I(f, t)
$$

This is a function:

👉 input: (fact, time)  
👉 output: true/false  

---

### In practice

$$
I(f,t) = (f \in S_t)
$$

---

### MODEL ($\mathcal{M}$)

$$
\mathcal{M} = (S_0, S_1, \dots, S_T)
$$

This is:

👉 the whole history of the world

---

# 5. ACTIONS (MOST IMPORTANT PART)

$$
A = (Pre(A), Add(A), Del(A))
$$

Each action has:

---

### Pre(A)

What must be true to execute the action

---

### Add(A)

What becomes true after the action

---

### Del(A)

What becomes false

---

### Example

openDoor:

Pre = {$$\neg doorOpen$$}  
Add = {doorOpen}  
Del = $$\varnothing$$  

---

### Intuition

An action = function that changes the world

---

# 6. STATE TRANSITION (CRITICAL FORMULA)

$$
S_{t+1} = (S_t \setminus Del(Acs_t)) \cup Add(Acs_t)
$$

This is the MOST IMPORTANT equation.

---

### Step-by-step

1. Start with current state  
2. Remove things that actions delete  
3. Add things that actions create  

---

### Example

$$
S_t = \{\neg doorOpen\}
$$

Action:  
Add = {doorOpen}

Result:

$$
S_{t+1} = \{doorOpen\}
$$

---

# 7. CONCURRENCY (MULTI-AGENT)

$$
Acs_t = \{A_1, A_2, \dots, A_k\}
$$

Meaning:

👉 multiple actions can happen at the same time

---

### We combine

$$
Add = \bigcup_i Add(A_i)
$$

$$
Del = \bigcup_i Del(A_i)
$$

---

### Intuition

Many agents modify the world simultaneously

---

# 8. INERTIA (VERY IMPORTANT)

$$
(f \in S_t) \land (f \notin Del(A)) \Rightarrow (f \in S_{t+1})
$$

Meaning:

👉 if something is NOT changed → it stays the same

---

### Why important

Without this:  
you would need to rewrite the whole state every time

---

# 9. ACTION LANGUAGE ($\mathcal{L}_A$)

This is a language to DESCRIBE the system

---

### holds(f, t)

Is fact $f$ true at time $t$?

---

### occurs(A, t)

Did action $A$ happen?

---

### executable(A, t)

Can action $A$ be executed?

---

### Semantics

$$
\mathcal{M} \models holds(f, t)
$$

Means:

👉 "in this model, $f$ is true at time $t$"

---

# 10. QUERY LANGUAGE ($\mathcal{L}_Q$)

This is a language to ASK QUESTIONS

---

### Q1

$$
holds(C, t)
$$

Is condition $C$ true at time $t$?

---

### Q2

$$
involved(agent)
$$

Did this agent perform any action?

---

### Intuition

Action language = describe system  
Query language = ask questions  

---

# 11. REASONING PROCESS

We simulate the world step by step:

$$
S_{t+1} = update(S_t)
$$

---


### In code

```python
for t in time:
    S[t+1] = transition(S[t])
````

---

# 12. HOW TO IMPLEMENT IN PYTHON

---

### STATE

```python
state = {"doorOpen", "lightOn"}
```

---

### ACTION

```python
class Action:
    def __init__(self, pre, add, delete):
        self.pre = pre
        self.add = add
        self.delete = delete
```

---

### TRANSITION

```python
def apply_actions(state, actions):
    new_state = state.copy()

    for a in actions:
        new_state -= a.delete

    for a in actions:
        new_state |= a.add

    return new_state
```

---

### SIMULATION

```python
states = [S0]

for t in range(T):
    next_state = apply_actions(states[t], actions_at_t)
    states.append(next_state)
```

---

### QUERY

```python
def holds(state, C):
    return C.issubset(state)
```

---

# 13. BIG PICTURE (MOST IMPORTANT)

Your entire system is:

👉 set of facts

👉 + actions that modify sets

👉 + time loop

👉 + query checking

That’s it.

---

### Mathematics = formal version of code

Sets → Python sets
Union → `|`
Difference → `-`

