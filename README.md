# my_blog


# 23/09/2024 

I'm back in a writing mood. I want to ramble into the void. I see my writing presence on the internet as transient. Here for a brief moment and then gone~ 
Its the reason why I don't wish to blog on a dedicated website for that sort of thing. You know I don't even read other people's blogs - too boring. Well I usually read 1-2 posts but I don't "follow" anyone. And you need to draw me in with some pictures and diagrams, not just words!
I think people think I read a bunch or whatever, I don't. I read 1 book this year so far - The Brothers Karamazov. Other than that, its mostly snippets of wikipedia articles, random internet comments that espouse lots of wisdom, and...explanations for whatever concept I'm trying to learn, which is just more wikipedia and random internet articles and videos. 

Lots of gold on the internet still if you know what to look for. But don't get too lost in the sauce. And don't fall for personalities. I don't care who or where the info comes from as long as its good. Good is subjective. To me, good = not boring, and bad = boring. boring = anything I don't like....I don't know what I don't like. Maybe I don't like anything. Maybe I like everything. 


These days its best to write mountains of nonsense on the internet. Mask intentions in coded language. words, words, words. I want to poison the data used to train the LLMs. 


What did I want to write about today? I can't remember.  
Its almost 1 year since I started working on my MMO game project. I didn't think I would get this far. If I showed a pic of what the game looks like to past-me, he would not beleive it. Its easy to lose sight of the work you are doing when you see it everyday. But also there is still so much work to do. SO much work. I don't want to think about it. Its better to remain delusional and ignore it than think too deeply about it. But also I can't remain delusional forever because I'm running out of money..hahaha. Still I'm very optimistic. I am trying to get back into the groove. Because my momentum has been good but it got derailed the past few weeks. And its making me distraught. So its time for renewal. No I wasn't procrastinating, it was all part of the plan! RENEWAL. I cast my old self down and emerge again. 

# 24/09/2024

wOWOW I am getting back into the mood. I was so confused last week, so lost, because I couldn't make sense of this dynamic world spawning system I'm trying to implement. The problem is 2 fold, it involves for one, integrating a new spawn queue sub-system into the existing architecture and figuring out how it should coordinate with the AI director + World Simulation to dynamically spawn new units into the world - what pre-conditions are needed, what is the polling frequency?, etc. And 2, figuring out how to represent the knowledge the AI director needs to make strategic decisions. When should it spawn new squads of enemies? How does it know where to send squads? How does it know which areas are contested or occupied? This part was really bugging me. I knew part of the answer was in Valve's Left 4 Dead AI Director implementation. And now after some further reasearch (watching GDC talks), I found exactly what I was looking for. The answer is: Influence Maps. Its the missing link! And Influence Maps let you do SO MUCH if properly implemented. This is exactly what I wanted. 

It makes a lot of sense. The AI director sees the world in terms of abstract strategic objectives, represented by nodes in a list. And these nodes also contain data used to signal things like priority, visibility, and adjacency to the AI director. And we can enforce some cost function to calculate the cost of traversing the nodes so that the "best choice" is made, w.r.t the current world conditions. Graphs and A*. Graphs and A*. Graphs and A*. Repeat it 500 times and you will acheive enlightenment. 


# 27/09/2024

OK I'm finally done planning. Now its time to start programming again. Its like that saying, spend time sharpening the axe so you can cut the tree down in one strike. yeah, I sharpen the axe while also being anxious the entire time about not cutting the tree. but why? the tree WILL be cut down. don't worry about it. keep sharpening. But what if you sharpen forever ..never cutting down the tree? what am i even saying.

What did planning involve? It involved slipping into a different headspace and staring at grids for several hours. And then going on long walks as a way to procrastinate. Just look at grids bro. Just think about Euclidean spaces bro. Stare into the abyss long enough and it will stare back. 


![zone_concept_v5](https://github.com/user-attachments/assets/056a2647-cce6-4d66-9f86-4e4bc6e38663)


The world is partitioned into a lattice. Connections between lattices form larger hexagonal regions. The hexagons form a packing (? I think). Influence propagates within a region, and if it exceeds a certain threshold it propagates along the region connections back to home base. The numbers along the connections don't represent path costs, those are calculated differently. Rather, they represent available resource allocation. I refuse to elaborate any further. You will see what happens in the game. 

ON ANOTHER NOTE. I'm coming up on 6 months of being smoke-free. Its a lie, its not like I had 0 cigarettes in this span of time. BUT, of the handful of times I had one I didn't feel the need to start again. It was like a one-and-done type of thing. I think I'm over it. Smoking is BORING. There I said it. 
BUT now its getting cold again....maybe I'll smoke for the month of December. 


# 01/10/2024

FINALLY, I finished implementing the knowledge map representation shown above. ALSO, all the communications, interfaces, data-exchange, etc between the World and the AI Commander (previously AI Director) is complete! The initial test simply involved booting up a new world server with an attatched AI Commander which receives the knowledge representation at world tick T0, looks for all the territories it currently controls, and then spawns squads of units into the territories. At T0 the goblin commander owns 7 out of the 9 world territories. The other 2 territories are player spawn zones that cannot be captured. Squad sizes can be varied - right now a squad consists of 36 units. 36 is a magic number...I have no clue if there will be a standard squad size or if it will vary with the game conditions.  One thing to note - the commander is initialized at world startup before the player connection queue is online and accepting new connect requests. So all of this is guaranteed to occur before any players can start spawning into the world! The implemented spatial partitioning algorithm partitions the world into 9 *territories* and 16 *control points*. Influence propagates from events occuring at control points. Gaining ownership of control points begins the process of gaining ownership of attatched territories. Control points are always found at the intersection between 2 territories. Territories are represented by hexes, control points are points with a radius. There are areas on the map that do not fall under a territory or within a control point radius. These areas are "anarchy zones", whatever actions players take here will not draw the attention of the AI commander. So its where you go to kill each other without being interrupted by a horde of goblins to take advantage of the chaos. But you should not kill each other! Its bad, bad, bad. You will be marked as a sinner. 

NOW its time for the fun part - implementing the AI commander descision loop. The goal of the AI commander is to poll the state of the world territories and control points at some interval, make sense of what is going on, and then to *execute a strategy*. Executing a strategy involves identifying a control point or territory under attack, finding nearby squads, and assigning objectives to the squad. Once a strategy is set in motion, the commander measures the result via looking at certain metrics (units lost, enemies remaining, etc). If things are not going well, a new strategy is undertaken, otherwise, the existing strategy continues to be executed. Squads that are far from the action - in distant territories not bordering contested objectives, are not assigned objectives. They remain idle, not consuming bandwidth or processing, eagerly awaiting the day they will be useful. What is a commander objective? Its simply a list of goals assigned to the AI Squad units. An obbjective like "defend territory X" corresponds to a set of goals like [patrol(X), attack_targets(:players)]. 


This sort of reminds me of my high-frequency trading system. It was the same idea. You look at the current conditions over some time horizon, if things look good you execute a strategy to BUY or SELL assets. Then you measure the result of your active strategy. If things are going bad you EXIT quickly and start the process of formulating a new strategy, otherwise you continue executing the existing strategy. 


# 02/10/2024

Moving quickly again. I've been writing new code and refactoring old stuff. Going back and refactoring code is such a cognitive load. But the effort always ends up being much easier than the expectation. Why? Because Elixir is that GOOD. The refactoring consisted of wrapping up the old interfaces to spawn AI into higher order abstractions called by the AI commander. Also the spatial partioning scheme for the strategy map has changed. The hex idea I was trying is cool, but I found a better way to do things. Actually the hex idea was stupid, but atleast it got me to start drawing stuff. Just keep drawing stuff! It will give you the solution. Yeah I think the next few days will be fun - it will be 50% coding, 50% playing the game. Heres my metric - if I find myself playing the game for longer than 5 minutes I'll know what I have is good enough for a first draft. I need to set a limit or I'll spend an infinite amount of time tweaking the commander just to see what will happen. Its like doing map editing for this game. I get sucked in, hours go by, entire days even, and important tasks become neglected. 

I think this period of blogging is coming to an end. Its served its purpose. Writing is boring again. Coding is fun. 





