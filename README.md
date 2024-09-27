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
