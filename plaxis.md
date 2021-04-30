- [1. INTRODUCTION](#1-introduction)
  - [1.1. Boilerplate](#11-boilerplate)
    - [1.1.1. Configure Remote Scripting](#111-configure-remote-scripting)
    - [1.1.2. Boilerplate Code for Inputs](#112-boilerplate-code-for-inputs)
    - [1.1.3. Boilerplate Code for Outputs](#113-boilerplate-code-for-outputs)
    - [1.1.4. Easiest Way to Start](#114-easiest-way-to-start)
  - [1.2. Creating a New Model](#12-creating-a-new-model)
# 1. INTRODUCTION

## 1.1. Boilerplate

Boilerplate is a piece of code to connect the Python with Plaxis. It uses `plxscripting` module developed by Plaxis. 

However, to be able to connect the Python to Plaxis, we have to request Plaxis to welcome any incoming connection. We can do that by *starting a server*. More information about boileplates can be found in this [link](https://communities.bentley.com/products/geotech-analysis/w/plaxis-soilvision-wiki/46005/using-plaxis-remote-scripting-with-the-python-wrapper) and below.

### 1.1.1. Configure Remote Scripting

To start a server, we will use following steps:
* Start Plaxis Input
* From the toolbar, go to *Expert*
* Go to *Configure Remote Script Server*
* Default local host server for inputs is 10000, you can leave it in default if you do not have a choice. Set a password. For all tutorials, we will use `123456`.


<figure>
  <img src="images/PlaxisConfigureRemoteScripting.png" style="width:100%">
  <figcaption>Configured View of the Remote Scripting Server</figcaption>
</figure>

### 1.1.2. Boilerplate Code for Inputs

There are two types of variables after connection to Plaxis. **s** and **g**. `s`represents the server and `g` represents the global object. Without going into the detail about these, we should say that the main tool that we will use is `g` since we will mostly do changes on the objects. However, sometimes we may need to start a new model, in these cases, we will use `s`.

Boilerplate for input looks like this. `s_i`and `g_i` represents the outcome of the connection and `i` represents the `input`.

```python
from plxscripting.easy import *
localhostport_i = 10000
password = "123456"
s_i, g_i = new_server("localhost", localhostport_i, password=password)
```

For this code to work, we need to have the `plxscripting` package in our PATH or we have to start the code from the Plaxis using the Editor or Interpreter options.
 
### 1.1.3. Boilerplate Code for Outputs

If you make this request to localhost where Output is started, it will return us output objects.

```python
from plxscripting.easy import *
localhostport_o = 10001
password = "123456"
s_o, g_o = new_server("localhost", localhostport_o, password=password)
```

### 1.1.4. Easiest Way to Start

Many of configurations can be done on the road. However, it is important to start from somewhere, you have to see that Python codes interact with the model. To do this open **Interpreter** from the Expert menu.

Plaxis *currently* uses Jupyter QtConsole, a really simple console tool to *just directly writing the code*. 

After starting QtConsole, just write `s_i.new()` which will start the new model. 

>How it worked without using the boilerplate?

It worked because we have started a console / IDE directly from Plaxis and it comes with pre-existing connection. You don't have to do anything, just write the codes. It is great for fast coding during model creation.

<figure>
  <img src="images/QtConsoleStart.png" style="width:100%">
  <figcaption>QtConsole Start</figcaption>
</figure>

## 1.2. Configuration of a New Model

Simple. Use the server object for input as we did [above.](#113-boilerplate-code-for-outputs)

```python
s_i.new()
```

### 1.2.1. Project Properties

Project Name:
```python
g_i.Project.Title = "Title of the New Project" 
```
Project Company:
```python
g_i.Project.Company = "Title of the New Project" 
```

## 1.3. How Can We Find the Available Commands?

There are couple of *methods* to be used to determine the available methods of a command. For example, `g_i.Project` acts as a general command for that. If we use, following command, it will print many of the available commands:

```python
g_i.Project.Company = "Title of the New Project" 
```

