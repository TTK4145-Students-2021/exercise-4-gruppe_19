Exercise 4: From Prototype to Production
========================================

1. Don't overengineer.
2. Always design properly.
3. Minor detail change will ruin your perfect model.
4. Always prototype first.
5. You will only know what you didn't know on the second iteration.
6. There is only budget/time for the first version.
7. The prototype will become production.

This exercise consists of two parts:

 1. A project design hand-in, where you explain your plans for networking in the project    
 1. A practical milestone for the project, where you are able to send some custom data structure across a network

You are not expected to have a "final" design, or to implement your design fully. Consider this as a "first draft", or "version 0.1".

Part 1: Network design hand-in
------------------------------

*This part of the exercise should be handed in as a group.*

Produce a reasonably-sized document with considerations on these questions, your proposed solutions, and other reflections on the networking portion of the project:

 - Guarantees about elevators:
   - What should happen if one of the nodes loses its network connection?
The network module will be design such that is can operate perfectly fine without it. It is not designed with unnecessary nodes, and the logic of the elevator’soperation will be embedded in the elevator nodes. The node that loses connection will try to reconnect and report the error such that the user is well aware. 
   - What should happen if one of the nodes loses power for a brief moment?
This will not affect the rest of the system, since it will only increase the operation time if on elevator is down. 
   - What should happen if some unforeseen event causes the elevator to never reach its destination, but communication remains intact?
It will retry. If it still doesn’t work, it will send the floor request to the elevator with the second lowest estimated time to fulfill all its request. 
   
 - Guarantees about orders:
   - Do all your nodes need to "agree" on an order for it to be accepted? In that case, how is a faulty node handled? 
No just one of them. It is although preferred that all agree for the request to be distributed the most sufficient. 
   - How can you be sure that a remote node "agrees" on an order?
If all nodes are well operating the system will be designed such that there will be no logic clash for each node. If this still isn’t the case it. 
   - How do you handle losing packets containing order information between the nodes?
The messages will be braodcasted so if one message is lost there is no big deal. 
   - Do you share the entire state of the current orders, or just the changes as they occur?
     - For either one: What should happen when an elevator re-joins after having been offline?
The nodes for the bottom pushed outside the elevator will send the request to all the elevators. The elevators will send a float variable with the estimated time to be fulfil the orders including the one requested to each other. Further the one with the lowest estimated time will accept the request. When a node is being offline and therefore don’t send its estimated time to the other elevators, it will be assumed it is unable to reach and one of the other elevators will take the request. If it re-joins it will send its estimated time as per usual and work perfectly fine with the rest of the system. 

*Pencil and paper is encouraged! Drawing a diagram/graph of the message pathways between nodes (elevators) will aid in visualizing complexity. Drawing the order of messages through time will let you more easily see what happens when communication fails.*
     
 - Topology:
   - What kind of network topology do you want to  implement? Peer to peer? Master slave? Circle? Something else?
The topology that is using is Peer to peer. This often gives a more complex node relationship but makes every node more independent
   - In the case of a master-slave configuration: Do you have only one program, or two (a "master" executable and a "slave")?
     - How do you handle a master node disconnecting?
     - Is a slave becoming a master a part of the network module?
Not master slave
   - In the case of a peer-to-peer configuration:
     - Who decides the order assignment?
Every elevator by communication. More details are provided in previous question. 
     - What happens if someone presses the same button on two panels at once? Is this even a problem?
The floor bottoms will have separated addresses, and there is a hierarchy of the periodization of handling the floor request and accepting the floor request, in case of such collision as bottom pressed at the same time, or same estimated time on two elevators. 
     
 - Technical implementation and module boundary:
   - Protocols: TCP, UDP, or something else?
      - If you are using TCP: How do you know who connects to who?
        - Do you need an initialization phase to set up all the connections?
      - If you are using UDP broadcast: How do you differentiate between messages from different nodes?
The messages is structs converted into a Json file. This will include an ID of the sender. 
      - If you are using a library or language feature to do the heavy lifting - what is it, and does it satisfy your needs?
There is no specific plan do use any library except the standard library that Go provides but this may change through the semester. 
   - Do you want to build the necessary reliability into the module, or handle that at a higher level?
Yes we want to build much of it in the module to make the code deciding how the elevator behaves as clean as possible. 
   - Is detection (and handling) of things like lost messages or lost nodes a part of the network module?
No, the messages broadcast so if one message gets lost it is not a big deal. 
   - How will you pack and unpack (serialize) data?
     - Do you use structs, classes, tuples, lists, ...?
We would use structs. This a structured way to include all the information needed in the message. We don’t need the private property classes provides in this case. 
     - JSON, XML, plain strings, or just plain memcpy?
Json. This is because it is great recourse of how to do this using go. 
     - Is serialization a part of the network module?
Yes, it is.


Part 2: Getting networking started
----------------------------------

By the end of this exercise, you should be able to send some data structure (struct, record, etc) from one machine to another. How you achieve this (in terms of network topology, protocol, serialization) does not matter, but you should try to keep it as close to your (preliminary) design as possible.

Some basic network modules already exist for several programming languages. Use whatever you find useful - extend, modify, or delete as you see fit.

 - [C network module](https://github.com/TTK4145/Network-c)
 - [D network module](https://github.com/TTK4145/Network-D)
 - [Go network module](https://github.com/TTK4145/Network-go)
 - [Rust network module](https://github.com/edvardsp/network-rust)
 - [Distributed Erlang](http://erlang.org/doc/reference_manual/distributed.html)
 
Since this is the start of programming your project, you should start using your project repository on GitHub. If you find that this exercise becomes "short", keep working on your design, free-form programming, or move on to the next exercise.
 
### Running from another computer

In order to test networking on the lab, you may find it useful to run your code from multiple machines at once. The best way to do this is to log in remotely. Remember to be nice the people sitting at that computer (don't mess with their files, and so on), and try to avoid using the same ports as them.

 - Logging in:
   - `ssh username@#.#.#.#` where #.#.#.# is the remote IP
 - Copying files between machines:
   - `scp source destination`, with optional flag `-r` for recursive copy (folders)
   - Examples:
     - Copying files *to* remote: `scp -r fileOrFolderAtThisMachine username@#.#.#.#:fileOrFolderAtOtherMachine`
     - Copying files *from* remote: `scp -r username@#.#.#.#:fileOrFolderAtOtherMachine fileOrFolderAtThisMachine`
    
*If you are scripting something to automate any part of this process, remember to **not** include the login password in any files you upload to GitHub (or anywhere else for that matter)*

## Extracurricular

[The Night Watch](https://web.archive.org/web/20140214100538/http://research.microsoft.com/en-us/people/mickens/thenightwatch.pdf)
*"Systems people discover bugs by waking up and discovering that their first-born children are missing and "ETIMEDOUT" has been written in blood on the wall."*

[The case of the 500-mile email](http://www.ibiblio.org/harris/500milemail.html)
*"We can't send mail farther than 500 miles from here," he repeated. "A little bit more, actually. Call it 520 miles. But no farther."*

[21 Nested Callbacks](http://blog.michellebu.com/2013/03/21-nested-callbacks/)
*"I gathered from these exchanges that programmers have a perpetual competition to see who can claim the most things as 'simple.'"*
