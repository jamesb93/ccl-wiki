# Dynamic Max Patch Guide

### [Jacob Hart](https://jacob-hart.com)
---
# Introduction
Sometimes it can be useful to have a max patch construct itself depending on user input. This is especially true when sharing your patches with others, notably people who may not be as techno-fluent as yourself! In this guide, we shall discuss various techniques using _javascript_ to do this quickly and easily. We shal start by explained how to set up a dynamic max patch project, then look at a few interesting use-cases. Note that all of the code from this guide can be __[__downloaded here](https://github.com/jdchart/dynamic_max_patch_guide)__.

# Setup
We shall start by creating a new _max patch_ and a _javascript file_ in our project folder. As you can see in the image below, we're using _Visual Studio Code_ for writing the javascript file. We've started by setting the helper messages for the inlet and the outlet of the _js object_, and writing a test function that prints to the console and sends a bang out the first outlet whenever a bang is sent to the first inlet.

<img src="/img/dynamicMaxPatch/dynamic_max_patch_01.png" alt="Setting up our dynamic patch.">

## Reset Function
Once we know everything is working, we can start coding our dynamic max patch. The first function to write, is a __reset function__. This shall be a function that finds all of the objects created by our javascript and removes them from the max patch. This will give us control over which objects exist and which objects don't at any given moment.

```javascript
function reset(){
    to_delete = [];
    obj = this.patcher.firstobject;

    for(i = 0; i < this.patcher.count; i++){
        if(obj.varname.indexOf('jscreated') > -1){
            to_delete.push(obj);
        }

        obj = obj.nextobject;
    }

    for(i = 0; i < to_delete.length; i++){
        this.patcher.remove(to_delete[i]);
    }
}
```

In this function, we make a list of all the objects which have the substring _jscreated_ in their scripting name, then we iterate through this list and delete each object. Now, whenever we want an object to be removed on reset, all we have to do is make sure it has _jscreated_ in the scripting name!

## Object Creation function
Next we shall make a general __object creation function__ - this is the function we shall call whenever we want to create a new object. We shall pass the _x, y coordinates_ in the pacther to create the object, the _object name_ and an array of any _arguments_ the object may have. We shall also return the object in order to manipulate it quickly later, and give it the scripting name _jscreated_ so that it gets deleted by our reset function.

```javascript
function create_obj(x, y, object, args){
    obj = this.patcher.newdefault(x, y, object, args);
    obj.varname = 'jscreated';

    return obj;
}
```

In order to test that everything is working, create a quick test function that creates a few objects, then delete them like show in the image below. If everything is in order, we are ready to move on.

<img src="/img/dynamicMaxPatch/dynamic_max_patch_02.png" alt="Basic functions for a dynamic max patch.">

# Use-cases
Next we shall have a look at a few use-cases that make use of dynamic object creation and modification in interesting ways.

## Patch Styler
First, let's make a patch where you can __change the style of the objects within a patch__. This can be useful when you want to make modifications to lots of different objects at once. Let's setup up our patch and our _javascript_. As you can see below, we've made a simple patch with various elements that we wish to stylise. In the left panel, we shall put the commands for our _js_.

<img src="/img/dynamicMaxPatch/dynamic_max_patch_03.png" alt="Setting up our patch styler.">

First of all, we'll learn how to get **a specific object**, then change its attributes. Take a look at the following function:

```javascript
function set_button_bgcol(r, g, b, a){
    obj = this.patcher.getnamed("play_bang");
    
    obj.message('bgcolor', r, g, b, a);

    outlet(0, 'button_bg bgcolor ' + String(r) + ' ' + String(g) + ' ' + String(b) + ' ' + String(a));
}
```

We pass the _red, green, blue_ and _alpha_ values we wish our object to be. First of all, we find the object we wish to modify by using the _getnamed_ function. This looks for the object sith the given scripting name. When we build our patch, we called the big button *play_bang*. Then, we send this object a message using the _message_ function. With this function, you can send any object any message you would normally be able to send it - here we send the message _bgcolor_ which modifies the background colour, then pass the 4 arguments. This on its own will change the button.

Note that we have also added a message to send out of the first outlet: *button_bg bgcolor r g b a*. Look at the image below - you can see that we have routed this message to control the background colour of the panel object. Note the _deferlow_ and _sprintf %s_ objects : whenever we send messages out of the js outlet, it is good practice to pass them through these first. _Deferlow_ makes the message a low priority (useful to prevent crashing), and _sprint %s_ will remove the quotation marks added by max to our message. This shows that we can communicate with the objects in our patch wirelessly and with patch cords.

<img src="/img/dynamicMaxPatch/dynamic_max_patch_04.png" alt="Wireless and wired object communication.">

Next we shall see how to modify **certain types of objects**. Let us say that we want to be able to control the colour of all the panel objects in our patch. Consider the following function:

```javascript
function set_panel_bgcol(r, g, b, a){
    obj = this.patcher.firstobject;

    for(i = 0; i < this.patcher.count; i++){
        if(obj.maxclass == 'panel'){
            obj.message('bgcolor', r, g, b, a);
        }
        
        obj = obj.nextobject;
    }
}
```

Here, we iterate through all of the objects in the patch. Every object has a _maxclass_ that tells us the type of object it is. Here we ask our function to change the colour obly if the current object's maxclass is a _panel_. The maxclass of an object can sometimes be surprising - for example the _*~_ object maxlass is _times~_. You can always iterate through the patch's objects, or get an object by it's scripting name to find out what an object's maxclass is.

We can change a whole host of other things. In this next function, we shall choose to toggle between displaying only the button object, and showing everything:

```javascript
function show_objects(x){
    obj = this.patcher.firstobject;

    for(i = 0; i < this.patcher.count; i++){
        if(x == 0){
            if(obj.varname == 'play_bang' || obj.varname == 'show_toggle'){
                obj.hidden = false;
            }
            else{
                obj.hidden = true;
            }
        }
        else{
            obj.hidden = false;
        }
        
        obj = obj.nextobject;
    }
}
```

Here, when the argument x is 0, we go through every object, and set the _hidden_ property to true unless it's scripting name is *play_bang* or *show_toggle*. Let's consider one last function:

```javascript
function set_slider_width(x){
    obj = this.patcher.getnamed('slider');

    obj.message('patching_rect', obj.rect[0], obj.rect[1], x, 40);
    obj.message('orientation', 1);
}
```
Here we set the width of the slider by sending the *patching_rect* message, supplying the x, y, width and height arguments. We also make sure to set the _orientation_ to 1 so that it doesn't automatically switch to vertical. You can see that we can control a good number of things with javascript, notably controlling multiple objects without the need for wires!

## Object Attribute Viewer
Next, let's make a patch where you can select an object to create, and then __see all of the object's attributes__ and even modify them! This can be a good technique for learning about new objects. Let's first set up our project. As you can see, we've set up a *set_object* function to which we pass an object name. On the left is the place where the object attributes shall appear.

<img src="/img/dynamicMaxPatch/dynamic_max_patch_05.png" alt="Setting up our object attribute viewer project.">

The first step is to create our object, let's make use of our *create_obj* function:

```javascript
main_object_location = [326, 120];

function set_object(obj_name){
    reset();

    created_obj = create_obj(main_object_location[0], 
                            main_object_location[1], 
                            obj_name);
}
```

Note that we created a global variable, *main_object_location*, giving the object location. The next step is to get a list of all of the created object's attributes. To do this we need to create some more objects in the patch, as we shall be using the _getattr_ object. Consider the following function:

```javascript
main_object_location = [326, 120];
hidden_object_location = [457, 24];
spacing = 25;

function set_object(obj_name){
    reset();

    created_obj = create_obj(main_object_location[0], 
                            main_object_location[1], 
                            obj_name);
    created_obj.varname = 'jscreated_main_object';

    first_get_attr = create_obj(hidden_object_location[0],
                                hidden_object_location[1],
                                'getattr', ['@listen', 0]);
    first_get_attr.varname = 'jscreated_get_attr';

    route_obj = create_obj(hidden_object_location[0],
                            hidden_object_location[1] + (spacing * 1),
                            'route', ['attrlist']);
    route_obj.varname = 'jscreated_route';

    iter_obj = create_obj(hidden_object_location[0],
                            hidden_object_location[1] + (spacing * 2),
                            'iter');
    iter_obj.varname = 'jscreated_iter';

    prepend_obj = create_obj(hidden_object_location[0],
                                hidden_object_location[1] + (spacing * 3),
                                'prepend', ['add_attribute']);
    prepend_obj.varname = 'jscreated_prepend';
    
    this.patcher.connect(first_get_attr, 1, created_obj, 0);
    this.patcher.connect(first_get_attr, 2, route_obj, 0);
    this.patcher.connect(route_obj, 0, iter_obj, 0);
    this.patcher.connect(iter_obj, 0, prepend_obj, 0);
    js_obj = this.patcher.getnamed('js_ctrl');
    this.patcher.connect(prepend_obj, 0, js_obj, 0);
}
```

Here, we've created a whole series of objects. At the end, we have also used the _connect_ function to connect the objects as needed. We've also given them all different scripting names so that we can acces them quickly later on. Now when the user chooses an object, these objects shall be created and connected like in the image below.

<img src="/img/dynamicMaxPatch/dynamic_max_patch_06.png" alt="Objects created by our javascript.">

We'll put all of this into a *create_hidden_objects* function to keep our code segmented. We shall also make them hidden, and their patch cords hidden so that the screen doesn't get too cluttered. The next step is to get all of the object attribute - we shall do this by sending the _getattrlist_ message to the _getattr_ object. This shall trigger the objects we have just created, and send each attribute to the js object prepended by *add_attribute*. Let's create a global attribute list to populate, and the *add_attribute* function. Our code will look like this:

```javascript
attribute_list = [];

function set_object(obj_name){
    reset();
    attribute_list = [];

    created_obj = create_obj(main_object_location[0], 
                            main_object_location[1], 
                            obj_name);
    created_obj.varname = 'jscreated_main_object';

    create_hidden_objects();

    attr_obj = this.patcher.getnamed('jscreated_get_attr');
    attr_obj.message('getattrlist');
}

function add_attribute(attribute){
    attribute_list.push(attribute);
}

function post_attribute_list(){
    for(i = 0; i < attribute_list.length; i ++){
        post(attribute_list[i] + '\n');
    }
}
```

Now, if we send the message *post_attribute_list* to the js object, a list of the object's attributes shall be posted to the console, like in the image below.

<img src="/img/dynamicMaxPatch/dynamic_max_patch_07.png" alt="Attribute list output.">

The next step is to create an _attrui_ object for each attribute. We shall do this programatically with the following function:

```javascript
function create_attrui(){
    main_obj = this.patcher.getnamed('jscreated_main_object');
    panel_obj = this.patcher.getnamed('attribute_panel');
    last_object_height = 0;

    for(i = 0; i < attribute_list.length; i++){
        attrui_obj = create_obj(attriui_locations[0],
                                attriui_locations[1] + (i * spacing),
                                'attrui');

        this.patcher.hiddenconnect(attrui_obj, 0, main_obj, 0);
        
        attrui_obj.message('attr', attribute_list[i]);
        attrui_obj.message('patching_rect', attriui_locations[0], 
                                            attriui_locations[1] + (i * spacing), 
                                            panel_obj.rect[2] - panel_obj.rect[0], 22);

        last_object_height = attrui_obj.rect[3] ;
    }

    panel_obj.message('patching_rect', panel_obj.rect[0], 
                                        panel_obj.rect[1], 
                                        panel_obj.rect[2] - panel_obj.rect[0], 
                                        last_object_height);

}
```

Here we start by getting the _main object_ and the _attribut panel object_, and set a _variable_ that shall be used to set the height of the panel object later on. Next we iterate through the attribute list, and create an _attrui_ object for each attribute. We connect this object to the main object, then send it a message to set it to the current attribute. For each object, we then set it's width to be that of the attribute panel, and set the current *last_object_height* variable to the height of the current attribute object. Finally, we set the height of the _panel_ object to be that of the last _attrui_ object.

Now, if we send the *create_attrui* to the js object after we choose an object, a set of attributs should be created like in the following image.

<img src="/img/dynamicMaxPatch/dynamic_max_patch_08.png" alt="The created attrui objects.">

The final step shall be to make it so that the *create_attrui* command is triggered after we have chosen our object and obtained the attribute list. In order to do this we shall modify our *create_hidden_objects* function to add a *t create_attrui attrlist* object and a _deferlow_ object, and just send a _bang_ to trigger the process. We shall end up with the following setup.

<img src="/img/dynamicMaxPatch/dynamic_max_patch_09.png" alt="The final conriguration.">

Now everything shall happen automatically whenever the user chooses an item.

## JSON to Max
As a final use-case, we shall make an object that **constructs the max patch by reading data from a _JSON_ file**. This can be useful when you want to make dynamic patches with lots of different states. Let's start by making a function that reads the data from a _JSON_ file:

```javascript
function load_json(file){
    f = new File(file, 'read');
    f.open();
    json_string = '';
    while(f.eof > f.position){
        line = f.readline();
        json_string = json_string + line;
    }
    f.close();

    return JSON.parse(json_string);
}

json_data = load_json('data.json');
```

We can put this at the beginning of our script so that the data is loaded from the start. Now lets create some data in our _JSON_ file. We'll create three different states that the user can choose between, each containing different parameters.

<img src="/img/dynamicMaxPatch/dynamic_max_patch_10.png" alt="The data for our first state.">

If you've ever opened a max patch in a text editor before, you may recognise a similar set-up. For each state, we are going to have a list of _objects_ and a list of _connections_, as well as a _name_ and a _description_. 

Each _object_ has 5 attributes: the _object name_, _arguments_ to pass when creating the object, a _position_ in the patch, a _scripting name_, and a list of _messages to send it on initialisation_. 

Each connection is a list, giving the _source object index_, the _source outlet_, the _target object index_ and the _target inlet_.

We can also set up our patch like this, ready for programming. We have to comment objects where we shall set the title and description, and a umenu connected to ou js object to pass in the patch we wish to display.

<img src="/img/dynamicMaxPatch/dynamic_max_patch_11.png" alt="The patch is ready to be used.">

Now that we have our data, we can create a function that builds the patch programatically. Consider this function:

```javascript
function load_patch(x){
    reset();

    if(x == 0){
        patch_key = "state_01";
    } else if(x == 1){
        patch_key = "state_02";
    } else if(x == 2){
        patch_key = "state_03";
    }

    name_comment = this.patcher.getnamed('patch_name');
    desc_comment = this.patcher.getnamed('patch_description');

    name_comment.message('set', json_data[patch_key]["name"]);
    desc_comment.message('set', json_data[patch_key]["description"]);
    object_list     = json_data[patch_key]["objects"];
    connection_list = json_data[patch_key]["connections"];

    created_object_list = [];

    for(i = 0; i < object_list.length; i++){

        args_list = object_list[i]["args"];
        init_list = object_list[i]["init"];

        obj = create_obj(object_list[i]["pos"][0], 
                        object_list[i]["pos"][1], 
                        object_list[i]["name"], 
                        args_list);  
        
        for(j = 0; j < init_list.length; j++){
            obj.message(init_list[j]);
        }
        
        created_object_list.push(obj);
    }

    for(i = 0; i < connection_list.length; i++){
        this.patcher.connect(created_object_list[connection_list[i][0]],
                                connection_list[i][1],
                                created_object_list[connection_list[i][2]],
                                connection_list[i][3])
    }
}
```

Let's go through it. We start by resetting as usual. Then we get the _patch key_ that the user has input. Next we find in our patch two existing objects - the _patch title object_ and the _patch descrition object_. We set their values by navigating through our _JSON_ data, inputting the key and then the keys for _name_ and _description_.

Next, to make things less cluttered, we assign the _object_ and _connection_ lists to their own variables. We also create an empty array which we shall populate with each of our created objects.

Next we iterate through each of our objects to create and do the following things: assign the _args_ and _init_ lists to their own variables; _create_ the object at the given _position_, passing the _name_ and the _args_, then we iterate through the _init_ commands and send the object each command; finally we add the created object to the array we made earlier.

At this point we have created all of our objects, all that's left is to connect them up. For this, we iterate through all of the _connections_, and use the *created_object_list* to refer to the objects we have made. Now when we select patch one (index 0) from the umenu, the patch should be created like in the image below.

<img src="/img/dynamicMaxPatch/dynamic_max_patch_12.png" alt="Our programatically created patch.">

As a final touch, we can add another key to each object called _init_, which can be a list of lists, giving an object index and a command to send once all of the objects have been created. Then we can add this following code to the end of our function:

```javascript
for(i = 0; i < json_data[patch_key]["init"].length; i++){
        created_object_list[json_data[patch_key]["init"][i][0]].message(json_data[patch_key]["init"][i][1]);
    }
```

Have a look at some of the other patches, and try and make your own!

# Conclusion
Hopefully you can now see the amount of things that you can do with javascript to make your max patches dynamic and user friendly. Remember you can always check the max reference for all of the functions available in _max javascript_.