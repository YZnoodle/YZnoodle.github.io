---
layout: post
title: Godot Json Dialogue Parser (in depth)
category: [code]
---
This dialogue parser created in Godot is reusable and allows quick creation of mini games. Json file can be easily parsed into dialogue texts using this tool. 

[<img src="{{ site.baseurl }}/images/demo/0220DialogueDemo.gif" style="width: 300px;"/>]({{ site.baseurl }}/images/demo/0220DialogueDemo.gif)

[Link to Github](https://github.com/YZnoodle/DailogueParser)  
<!--more-->

**How to use**   
1. Instance a parser
2. Add parser to tree   
3. Call parser with the conversation’s json file path  
4. Make buttons to show the parsed texts  
5. (Optional) If there are method calls associated with options, a signal call_emitted() will be emitted, first argument is a string of the method name. Methods need to be handled outside the dialogue manager.  
Then the user clicks and clicks until the conversation is over…  
Repeat step 3 and 4 for a new conversation.  

**Step by Step Example**  
Suppose we want to present this short conversation with branches:  
[<img src="{{ site.baseurl }}/images/graphs/0120graph1.png" style="width: 480px;"/>]({{ site.baseurl }}/images/graphs/0120graph1.png)
 
The first step is to write a json file. The json file will store this conversation in a special format. Some terminologies used in this design:  
- *Graph*: a conversation with an entry point and one or more exit point. It is the json file that the parser receives.  
- *Block*: a sequence of dialogues with no jumps or branches. A graph consists of one or more blocks. The first block must be named “start”.   
- *Dialogue*: the sentence or sentences to be displayed on screen at once. Such as “Hello”. A block holds one or more dialogues in a json array named “dialogues”.  

The .json file for this example looks like this:  
<details>
  <summary>Click to expand... </summary>
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

**Extention**  
Write a rule checker to check if a json file is valid.  
Most of the rules should be intuitive and easily maintained if started with a graphical design like in the example above:  
1. A graph must have a start block named “start”  
2. Each block must contain key "dialogues", which must be a list of size greater than 0  
3. Each object in "dialogues" must contain key "text", which must be a string  
4. If last object in "dialogues" have an "options" key, block must not be final AND number of objects in "options" must EQUAL number of strings in "next\_blocks"  
5. Each object in "options" must have a "key" and a "text" that are strings  
6. If last object in "dialogues" does not have "options" key,  block must be final OR "next\_blocks" has one and only one string member  

**References**  
Very useful website for creating json files:  
[https://jsoneditoronline.org](https://jsoneditoronline.org)

