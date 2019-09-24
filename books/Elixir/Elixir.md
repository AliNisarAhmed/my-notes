# Elixir 

## Built-in Types

#### Value types:
- Arbitrary-sized integers
- Floating-point numbers
- Atoms
- Ranges
- Regular expressions

#### System types:
- PIDs and ports
- References

#### Collection types:
- Tuples
- Lists
- Maps
- Binaries

### Atoms

- Atoms are constants that represent something’s name. We write them using a leading colon (:), which can be followed by an atom word or an Elixir operator.

- An atom word is a sequence of UTF-8 letters (including combining marks), digits, underscores, and at signs (@). It may end with an exclamation point or a question mark. 

- Examples: 
  ```elixir
    ​:fred​  ​:is_binary?​  ​:var@2​  ​:<>​  ​:===​  ​:"func/3"​ ​:"long john silver"​  ​:эликсир​   ​:mötley_crüe
  ```

- An atom’s name is its value. Two atoms with the same name will always compare as being equal, even if they were created by different applications on two computers separated by an ocean.

### Ranges 

- Ranges are represented as start..end, where start and end are integers.

### Regular Expressions

- Elixir has regular-expression literals, written as `~r{regexp}` or `~r{regexp}opts`.

### PIDs and Ports

- A PID is a reference to a local or remote process, and a port is a reference to a resource (typically external to the application) that you’ll be reading or writing.

- The PID of the current process is available by calling self. A new PID is created when you spawn a new process

### Tuples 

- A tuple is an ordered collection of values. As with all Elixir data structures, once created a tuple cannot be modified.

- You write a tuple between braces, separating the elements with commas.
​ 
  ```elixir 
    { 1, 2 }      { ​:ok​, 42, ​"​​next"​  }   { ​:error​, ​:enoent​ }
  ```

- A typical Elixir tuple has two to four elements—any more and you’ll probably want to look at maps,, or structs,.

- We can use tuples for pattern matching 

  ```elixir 
      >{status, count, action} = {​:ok​, 42, ​"​​next"​}
      >{:ok, 42, "next"}
  ```

- It is common for functions to return a tuple where the first element is the atom :ok if there were no errors. 

### Lists 

- Effectively Linked lists - easy to traverse, not so efficient to access data.

  ```elixir 
    # Some examples of functions on lists
    [ 1, 2, 3 ] ++ [ 4, 5, 6 ]
    [1, 2, 3, 4] -- [2, 4] # diff -> [1, 3]
    1 ​in​ [1,2,3,4] # true
    ​"​​wombat"​ ​in​ [1, 2, 3, 4] # false
  ```

### Keyword Lists 

Because we often need simple lists of key/value pairs, Elixir gives us a shortcut. If we write

  ```elixir ​ 
  [ ​name:​ ​"​​Dave"​, ​city:​ ​"​​Dallas"​, ​likes:​ ​"​​Programming"​ ]
  # It is converted to 

  [ {​:name​, ​"​​Dave"​}, {​:city​, ​"​​Dallas"​}, {​:likes​, ​"​​Programming"​} ]
  ```

- Elixir allows us to leave off the square brackets if a keyword list is the last argument in a function call. Thus,
​ 
  ```elixir 
    DB.save record, [ {​:use_transaction​, true}, {​:logging​, ​"​​HIGH"​} ]
    # can be written more cleanly as
    DB.save record, ​use_transaction:​ true, ​logging:​ ​"​​HIGH"vf
  ```

### Maps

- A map is a collection of key/value pairs. A map literal looks like this:

  ```elixir
    %{ key => value, key => value }
    states = %{ ​"​​AL"​ => ​"​​Alabama"​, ​"​​WI"​ => ​"​​Wisconsin"​ }
    responses = %{ { ​:error​, ​:enoent​ } => ​:fatal​, { ​:error​, ​:busy​ } => :retry​ }
    colors = %{ ​:red​ => 0xff0000, ​:green​ => 0x00ff00, ​:blue​ => 0x0000ff }
    # below, all the keys are different
    %{ ​"​​one"​ => 1, ​:two​ => 2, {1,1,1} => 3 }
    # If the key is an atom, you can use the same shortcut that you use with keyword lists:​ 
    colors = %{ ​red:​ 0xff0000, ​green:​ 0x00ff00, ​blue:​ 0x0000ff }
    # You can also use expressions for the keys in map literals:
    name = ​"​​José Valim"​
  ​  %{ String.downcase(name) => name }
    > %{"josé valim" => "José Valim"}
  ```

- In the first case the keys are strings, in the second they’re tuples, and in the third they’re atoms. Although typically all the keys in a map are the same type, that isn’t required.

- Why do we have both maps and keyword lists? Maps allow only one entry for a particular key, whereas keyword lists allow the key to be repeated. Maps are efficient (particularly as they grow), and they can be used in Elixir’s pattern matching,

- In general, use keyword lists for things such as command-line parameters and passing around options, and use maps when you want an associative array.

#### Accessing a Map

- You extract values from a map using the key. The square-bracket syntax works with all maps:

  ```elixir 
    states = %{ ​"​​AL"​ => ​"​​Alabama"​, ​"​​WI"​ => ​"​​Wisconsin"​ }
    > %{"AL" => "Alabama", "WI" => "Wisconsin"}
    states[​"​​AL"​]
    > "Alabama"
    states[​"​​TX"​]
    > nil
    # If the keys are atoms, you can also use a dot notation:
    colors = %{ ​red:​ 0xff0000, ​green:​ 0x00ff00, ​blue:​ 0x0000ff }
    colors[​:red​] 
    > 16711680
    colors.green 
    > 65280
  ```

- **NOTE**: You’ll get a `KeyError` if there’s no matching key when you use the dot notation.