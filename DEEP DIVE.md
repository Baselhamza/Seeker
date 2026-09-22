1.if there is high throughput of orders on the system we  must handle that number without letting our db to be overloaded 
we can use a virtual queue redis sorted list{priority queue} that always give the priority to the frist customer in buying and order service


2.using search elastic in searching for poducts to reduce the latency  and make in memory data base and used cdc to keep the data cosistant with the main postgress data base

3. in order service we used cache to handle the load to many writes in the surges of peak events like white black friday and used PATCH UPDATE TO HANDLE HIGHTHROUGHPUT WRITES in the main data base to keep the data consistance


2.if there is s high demand on sepcific order we must make it fast and easy to read and write in it so we need in memory db  like redis 
and connect it to the main db using

3.the best way to follow the condition of the robots is using heartbeat (ping pong) protocol
so the system has an real time updated status for them

4.Consistency for making orders same order is never processed twice we can use idempotency Key

5.two orders can never claim the same last unit we use the queue for handling orders 

6. what if some user made an order but never confirmed it or left it for a long time we need to use a Distributed Lock + TTL
for this order so we reserve the product in the current product for ttl 10 min then if the user did not confirm it we let the products go so somebody else can buy them (when the first person make an order in the buying phase the data updated int the redis memoery and when it's the turn of the next person he will find that the product is out of stock
but if the frist person did not confirm his order thin a 10 mintus the lock in the order will be unlocked and the product will back in stock again

7.some scaling issue is if we horizantly scaled the order service for example in this case we will need to add redis pup sub to make it easy for the robot service to find the order by its id easly
