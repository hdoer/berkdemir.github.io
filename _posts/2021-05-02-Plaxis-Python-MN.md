---
layout: post
title: Plaxis-Python Connection - MN Interaction
---



Recently, I have prepared a Moment-Axial Force Interaction Diagram that fetches the structural forces from Plaxis automatically. Using the amazing Streamlit module, I have created a simple GUI for MN diagram and published its video in Linkedin. The response was amazing and I got a lot of questions regarding the procedure, Python-Plaxis connection and Streamlit.

You can see the original post here.

<iframe src="https://www.linkedin.com/embed/feed/update/urn:li:ugcPost:6771757594685046784?compact=1" height="400px" width="100%" frameborder="0" allowfullscreen="" title="Embedded post"></iframe>

I will not publish the code since it will require me to check every aspect of the code, do extensive tests and prepare a documentation. Instead, I want to give some insights on the methods that I have used in the code. I had to try and fail too many times and contacted Plaxis support several times. Since this is a gray area still with lot to develop, it is not easy to find discussions on the internet, so even a brain-storming with Plaxis support is really valuable. (Thanks to Stefanos)

So, to create a record of these functions, I will share small gists (a little code snippets sharing tool by Github.)

# How to Get the Loads From Plaxis?

I have though about this a lot: What would be the best user experience? The best would be to know which plates user selected and get the loads for that plates. However, it is not possible with our current tools. So, I have created two options:

* **Groups**: User may group the plates beforehand. For example, 35_cm_shotcretes and 25_cm_shotcretes can be grouped together to seperately evaluate the structural capacity of these linings. 
  * **Disadvantage:** Tunnels designed in tunnel designer cannot be grouped in Plaxis (at least for now.) So, you will have to create the tunnel manually to use this option.
* **Plates:** This is not ideal, but it became the one I use mostly. All plates are listed to the user and user can select the plates that s/he want to plot. 
  * **Disadvantage:** Number of plates for complex geometries may increase to 10s, 20s with the intersection of temporary stages with the permanent wall. So, selecting too many plates can be inconvenient. 

We can fetch groups and plates from Plaxis. In my procedure, I create two list:

* **Group or Plate List:** Group or plate objects of Plaxis. They are not readable by user but instead they are called Entities with long codes. However, we have to use these objects to make necessary processes.
* **Group or Plate Names:** Names of groups or plates as seen in Plaxis GUI. 

An example output of `group_list_func()` given below.

`([<Entity {073DA50B-22A0-4D85-B9B1-E6470DC89EC4}>], ['RetainingWall'])`



## Disclaimer

I am not a coder, software engineer. So, there are many don't-dos in the codes below. For example, no proper error handling is used. The proposed codes' runtime can be decreased significantly. You should rewrite everything given below to have a proper code. If you do not know Plaxis-Python connection but you are experienced in Python, you can find clever ways to rewrite the code, but this article will help you to get started. If you find a better way to write these codes, please let me know and I can edit the Gist and post by thanking you.

## Before the Codes

There are several ways to contact with Plaxis using Python. One of them is using the interpreter coming built-in with Plaxis for short codes. You can reach this by Expert - Python - Interpreter menu. This is already connected to Plaxis without a need for `boilerplate` code that calls for the `plxscripting`. It uses Jupyter QtConsole and works great.

However, for longer codes, you will need to call for `plxscripting.easy` and you should start a new server with `s_o, g_o = new_server("localhost", localhostport_o, password=password)`. This is pretty standard although it seems complicated. See the tutorials on the Bentley Plaxis website for boilerplate codes. 

When we performed all these steps, we have four keys:

* s_i, g_i for Plaxis Input applications.
* s_o, g_o for Plaxis Output applications.

You will see that g_o is frequently used in the codes below since I mainly interact with output to fetch the results.

## Fetching Groups

Following function can be used to fetch the group list. Note that, no error handling is used, so if your model does not have groups, it will just print "Error."

<script src="https://gist.github.com/berkdemir/6519ee150bdbb3ee2aa1259b7254f116.js?file=BDPlaxisGroupList.py"></script>

## Fetching Plates

Same procedure can be adapted to plates too. 

<script src="https://gist.github.com/berkdemir/6b62233ab88631c0ac5e8bf864fe224e.js?file=BDPlaxisPlateList.py"></script>

## Phase List

You should also create a phase list to select the phases that you will get the loads from Plaxis. A simple for loop can be used or you can modify this code to select certain phases:

```python
Phase_list = []
Phase_names = []
for i in g_o.Phases:
    Phase_list.append(i)
    Phase_names.append(i.Identification.value)
```

## Fetching Loads

Now that we have everything (list of plates/groups and phases), we can ask for the loads from Plaxis.

Notice that, when the results are called using `g_o.getresults( object , phase,g_o.ResultTypes.Plate.M2D , "node")`, we have a list of forces that should be flattened using a simple list comprehension.

<script src="https://gist.github.com/berkdemir/6519ee150bdbb3ee2aa1259b7254f116.js?file=BDPlaxisGroupForces.py"></script>

<script src="https://gist.github.com/berkdemir/6b62233ab88631c0ac5e8bf864fe224e.js?file=BDPlaxisPlateForces.py"></script>

Note that resulting M and N lists are nested list due to incoming list types. I am 100% sure there are better ways to do it, mine was just to save the day. I used a flattener to re-write the list with something like `flatten = lambda t: [item for sublist in t for item in sublist]`. You can modify the code above to get a proper list.

**That's it!** Now that you have a list of M and N, so you can create your interaction diagram in another function and combine them!