# msgpack-nim

A MessagePack binding for Nim

[![Build Status](https://travis-ci.org/akiradeveloper/msgpack-nim.svg?branch=master)](https://travis-ci.org/akiradeveloper/msgpack-nim)

API: https://rawgit.com/akiradeveloper/msgpack-nim/master/msgpack.html

msgpack-nim currently provides only the basic functionality.
Please see what's listed in Todo section. Compared to other language bindings, it's well-tested by
1000 auto-generated test cases by Haskell QuickCheck, which always runs
on every commit to Github repository. Please try `make quickcheck` on your local machine
to see what happens (It will take a bit while. Be patient). Have a nice packing!

![Overview](https://rawgit.com/akiradeveloper/msgpack-nim/master/overview.svg)

## Install

```sh
$ nimble update
$ nimble install msgpack
```

## Example

```nimrod
import msgpack
import streams

# You can use any stream subclasses to serialize/deserialize
# messages. e.g. FileStream
let st: Stream = newStringStream()

assert(st.getPosition == 0)

# Type checking protects you from making trivial mistakes.
# Now we pack {"ints":[5,-3]} but more complex combination of
# any Msg types is allowed.
#
# In xs we can mix specific conversion (PFixNum) and generic
# conversion (toMsg).
let xs = @[PFixNum(5), (-3).toMsg]
let ys = @[(key: "ints".toMsg, val: xs.toMsg)]
st.pack(ys.toMsg) # Serialize!

# We need to reset the cursor to the beginning of the target
# byte sequence.
st.setPosition(0)

let msg = st.unpack # Deserialize!

for e in msg.unwrapMap:
  echo e.key.unwrapStr # emits "ints"
  for e in e.val.unwrapArray:
    # emits 5 and then -3
    echo e.unwrapInt
```

## Todo

* Implement Messagepack-RPC  
* Evaluate performance and scalability  
* Talk with offical Ruby implementation  
* Don't repeat yourself: The code now has too much duplications. Using templates?  

## Author

Akira Hayakawa (ruby.wktk@gmail.com)
