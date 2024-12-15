# my_blog


# 11/12/2024


A clean slate. I write these posts for *me*, not for *you*. OK so where am I with the game right now? I've been back on the grind after coming back from vacation. My motivation is high but my energy is low but thats a temporary problem. 
As always, things right now are very very very very very close. 2 more weeks guys! I'm finally closing out the remaining game client work, the UI is improved, the game client connection screen is legit, and skill hotbars are working and tied to the correct keybinds (one day it will be remappable). I should have emotes working by end of day. 
I'm also fixing up the game world, I know how to blend tiles between biomes better now so the transition looks smooth. Overall, I feel very confident on the Godot side of things. I really enjoy the Godot workflow. I have all sorts of customized reusable UI components now - panels, animated text, dynamic text, dynamic buttons, etc. Also browser performance is execellent. 

I can remember where I was exactly 1 year ago. I was in the process of just starting to implement the game client in Godot after I completed my proof of concept game client using HTML canvas. It was a period of great excitement and uncertainty. I was learning Godot and picking it up very fast, but suddenly a new entire domain opened up beyond the networking/server programming I was mostly doing. Art assets, animations, instantiation, TileMaps, etc. It was overwhelming but I ignored most of it and focused on keeping things as simple as possible. Being able to have a guy move across the screen in godot with animations playing was enough to make me happy. Of course now things have grown substantially in scope. But thats progress for you. We are in the big leagues now. 

Here is a comparision pic of how much things have progressed!

**December 2023**

![game_alpha](https://github.com/user-attachments/assets/49588d43-b716-46cc-9e23-06161a04afdd)

*(Left)* Newly implemented game client in Godot using simple tiles for the game world + placeholder assets for players and mobs. *(Right)* Initial proof of concept HTML game client.
The goal at the time was to use the HTML client as a reference on what to do in Godot. The HTML client was correctly synced with the server and could be used to test that server-side interest management was working correctly.

**December 2024**

![game_new_2024](https://github.com/user-attachments/assets/135bea5f-0f65-490f-b9ca-bf856d9c7169)

![game_new_death_2024](https://github.com/user-attachments/assets/9964c2c3-0bd6-4bc0-a6c9-5aa9a5609717)

So much progress. Customizable player characters + mobs with server side replication. Usable skills. Advanced Enemy AI. I can go on and on and on....


# 15/12/2024

BIG NEWS!! I've completed a bunch of UI + map work. Now I'm in the process of figuring out how to launch/deploy the game for public testing. I've been putting this part off for a while - but I've secured a route. I know how to do this! Its much easier than I thought. Last night I purchased a dedicated server for US-West. And now I have a *secret* tool that will allow me to put the server behind a reverse proxy for maximum security. No docker, nginx, server-admin required! And no changes to the server source code either. When you know what the workflow will be for deployment its a huge releif. 

Another thing I got working yesterday was the CDN. It was super easy. I'm using Cloudflare R2 for this. Much more straightforward than how I used to do it using AWS + S3 Cloudfront for my job. The CDN will be used to distribute the game files and host the static site for the game. 
By the way, I'm finding the process of setting up origin routing very easy in Cloudflare. 
