---
layout: post
title: Reusable json dialogue parser
category: [Code]
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
 

<details>
  <summary>The .json file should contain this: </summary>
  {  
  "start": {  
    "call": ["optional_method"],  
    "dialogues": [  
      {"text": "Hello"},  
      {  
        "text": "How are you?",  
        "options": [  
          {"text": "good"},  
          {"text": "bad"}  
        ]  
      }  
    ],  
    "next_blocks": ["a1", "a2"],  
    "is_final": false  
  },  
  "a1": {  
    "call": ["optional_method"],  
    "dialogues": [  
      {"text": "Good to hear"}  
    ],  
    "next_blocks": ["end"],  
    "is_final": false  
  },  
  "a2": {  
    "call": ["optional_method"],  
    "dialogues": [  
      {"text": "Ok"}  
    ],  
    "next_blocks": ["end"],  
    "is_final": false  
  },  
  "end": {  
    "call": ["optional_method"],  
    "dialogues": [  
      {"text": "Bye"}  
    ],  
    "is_final": true  
  }  
}  
</details>

**Rules**  
If there is time, I plan to write a rule checker to check if a json file is valid. But I want to test the parser out in a few projects first before committing too much time in this design because things might change.  
Most of the rules should be intuitive and easily maintained if started with a graphical design like in the example above.  
Here are the rules:  
1. A graph must have a start block named “start”  
2. Each block must contain key "dialogues", which must be a list of size greater than 0  
3. Each object in "dialogues" must contain key "text", which must be a string  
4. If last object in "dialogues" have an "options" key, block must not be final AND number of objects in "options" must EQUAL number of strings in "next\_blocks"  
5. Each object in "options" must have a "key" and a "text" that are strings  
6. If last object in "dialogues" does not have "options" key,  block must be final OR "next\_blocks" has one and only one string member  

**References**  
Very useful website for creating json files:  https://jsoneditoronline.org/ 
 









