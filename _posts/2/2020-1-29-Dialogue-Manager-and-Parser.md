---
layout: post
title: Reusable json dialogue parser design
category: Code
---
**Overview**  
This is a dialogue manager I created in Godot. The purpose of this dialogue manager is act as an easy tool to create small games and experiment with stuff quickly. The dialogue manager provides a simple dialogue GUI and a json parser.  

[<img src="{{ site.baseurl }}/images/graphs/0120overview.png" style="width: 480px;"/>]({{ site.baseurl }}/)

**How to use**  
At the start of the game:  
1. Instance dialogue manager  
2. Add dialogue manager to tree  
At a conversation:  
3. Call dialogue manager with the conversation’s json file path  
4. (Optional) If there are method calls associated with options, a signal call_emitted() will be emitted, first argument is a string of the method name. Method need to be handled outside the dialogue manager.  
Then user clicks and clicks until conversation is over…  
Repeat step 3 and 4 for a new conversation.  

**Json format design**  
Some terms I used to make this design:  
*Graph*  
A graph is a conversation with an entry point and one or more exit point. One json file equals one graph.  
*Block*  
A block is a sequence of dialogues with no jumps or branches.  
*Dialogue*  
A dialogue is the sentence or sentences to be displayed on screen at once. Such as “Hello”.  

**Example**  
Suppose we want to present this short conversation with branches:  
[<img src="{{ site.baseurl }}/images/graphs/0120graph1.png" style="width: 480px;"/>]({{ site.baseurl }}/)




