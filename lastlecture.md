# Last Lecture

## Eddie scrolls really fast rip, and I'm not really paying attention
See class lecture notes posted.

## Compare-Exchange
The following is a very fundamental operation, and all other operations on atomics (e.g. multiplication) can be implemented based off of ```compare_exchange```.
```
bool compare_exchange(std::tomic<T>* value, T* expected, T desired) {
  IN ONE ATOMIC STEP:
  if (*value == *expected) {
    *value = desired;
    return true;
  } else {
    *expected = *value;
    return false;
  }
}
```
 
MULTIPLYING BY 7:
```
int expected = x;
while (!compare_exchange(&x, &expected, &expected * 7) {
}
return expected * 7;
// cannot return x because it is possible that the atomic could have changed once the atomic operation (compare_exchange) ended
```

## Fairness
Create a ticket lock.
```
struct tkt {
  std::atomic<int> next = 0;
  std::atomic<int> current = 0;
}

assert(tl.next >= tl.current);
// locked if tl.next != tl.current

lock (tkt *tl) {
  int me = ++tl->next-1;
  while(me != tl.current) {
  }
}

unlock (tkt *tl) {
  ++tl->current;
}
```
