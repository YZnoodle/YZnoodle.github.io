---
layout: post
title: Cat Miner \: Recreating Gold Miner in Unity (in depth)
category: [code]
---

[<img src="{{ site.baseurl }}/images/demo/0220DialogueDemo.gif" style="width: 300px;"/>]({{ site.baseurl }}/images/demo/1008CMTitle.jpg)

<!--more-->

**Intro**   
Gold Miner has been one of my favourite game to pass time. It is simple enough, so I decided to recreate Gold Miner in Unity.  
In Gold Miner, a crane swings back and forth, and when the player hits the key, it will plunge down into the ground. The goal is to hit as much gold or jewel as possible with the crane's hook. 

**Simulating the Rope**  
In Gold Miner, player presses a key to let the crane dig down in the current direction:  
[<img src="{{ site.baseurl }}/images/graphs/0120graph1.png" style="width: 480px;"/>]({{ site.baseurl }}/images/graphs/0120graph1.png)
 
The first step is to write a json file. The json file will store this conversation in a special format. Some terminologies used in this design:  
- *Graph*: a conversation with an entry point and one or more exit point. It is the json file that the parser receives.  
- *Block*: a sequence of dialogues with no jumps or branches. A graph consists of one or more blocks. The first block must be named “start”.   
- *Dialogue*: the sentence or sentences to be displayed on screen at once. Such as “Hello”. A block holds one or more dialogues in a json array named “dialogues”.  

After you have created the .json file, instance parser.gd as a child of the main game node.  
Then to show the parsed text, either write your own gui using the parser’s API, or copy and integrate my main.gd to your main and or gui node.  
If you want to reference the existing main.gd, simply replace the following paths to your own:  

<pre><code>#Path to dialogue text button
onready var prompt_node = $"gui/screen/screen_content/prompt"
#Path to the parent node of the options container  
onready var dynamic_content_node = $"gui/screen/screen_content/dynamic_content"
#Path to json file
onready var json_path = "res://test.json"
</code></pre>


Done, the result should be similar to the demo at the very beginning of this post! 

**Rotation**  
Write a rule checker to check if a json file is valid.  
Most of the rules should be intuitive and easily maintained if started with a graphical design like in the example above:  
1. A graph must have a start block named “start”  
2. Each block must contain key "dialogues", which must be a list of size greater than 0  
3. Each object in "dialogues" must contain key "text", which must be a string  
4. If last object in "dialogues" have an "options" key, block must not be final AND number of objects in "options" must EQUAL number of strings in "next\_blocks"  
5. Each object in "options" must have a "key" and a "text" that are strings  
6. If last object in "dialogues" does not have "options" key,  block must be final OR "next\_blocks" has one and only one string member  

**Movement** 

**Deceleration** 

**Object Generation and Collision Prevention** 







