---
layout: post
title: Reusable json dialogue parser design
category: Code
---
**Overview** </br>
This is a dialogue manager I created in Godot. The purpose of this dialogue manager is act as an easy tool to create small games and experiment with stuff quickly. The dialogue manager provides a simple dialogue GUI and a json parser. </br>

[<img src="{{ site.baseurl }}/images/graph/0120overview.png" style="width: 480px;"/>]({{ site.baseurl }}/)

**How to use** </br>
At the start of the game: </br>
1. Instance dialogue manager </br>
2. Add dialogue manager to tree </br>
At a conversation: </br>
3. Call dialogue manager with the conversation’s json file path </br>
4. (Optional) If there are method calls associated with options, a signal call_emitted() will be emitted, first argument is a string of the method name. Method need to be handled outside the dialogue manager. </br>
Then user clicks and clicks until conversation is over…</br>
Repeat step 3 and 4 for a new conversation. </br>



