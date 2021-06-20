# Flex-Panel Image Documentation

<p align="center"> 
    <img src="demo.gif" width="500"> 
</p>


1. Currently, all the images on the page are in the default html layout. To manipulate the layout of our images more effectively, we will be using the CSS flexbox layout. 
    1. Inside of the ".panels" class selector, add the line display: flex. This will place all of our panels into a "flex container". The default behavior of a flex container, is to arrange all of the images in a side-by-side layout. Conveniently for us, this is our desired layout. 
    2. The panels are probably all squished together right now. To fix this, in the ".panel" class selector, add the line "flex : 1;". This will evenly space out all the panels. 
        - *Explanation:* "flex: " is shorthand for the "flex-grow, flex-shrink, and flex-basis" properties. In brief, these properties are assigned to flex items (items you placed in a flex container, which in this case was the panels) and essentially tells these items how much space they should allocate for themselves. The ".panel" items were children of the ".panels" item and were thus flex items in the flex container defined in ".panel". The lines "flex: 1" told all the panel children to allocate the same amount of space for themselves in the "panels" container. To read more: [https://developer.mozilla.org/en-US/docs/Web/CSS/flex](https://developer.mozilla.org/en-US/docs/Web/CSS/flex)

2. Now that we've set up our panels, it's time to set up our text within the panels. We will once again use CSS flexbox to accomplish this. 
    1. Within the ".panel" class selector, add the following code: 

        ```jsx
        display: flex; 
        justify-content: center; 
        align-items: center; 
        flex-direction: column; 
        ```

        - *Explanation: "*display: flex", just like the previous section, sets up a flex box except it's now set up in each panel. "justify-content: center" and "align-items: center" does exactly what the property says, it justifies and aligns the text to the center of each panel. "flex-direction: column" changes the default layout of the flex box from side-by-side to a top-to-bottom column layout. If you want to read more, check out the links below:
            - justify-content: [https://developer.mozilla.org/en-US/docs/Web/CSS/justify-content](https://developer.mozilla.org/en-US/docs/Web/CSS/justify-content)
            - align-items: [https://developer.mozilla.org/en-US/docs/Web/CSS/align-items](https://developer.mozilla.org/en-US/docs/Web/CSS/align-items)
            - flex-direction: [https://developer.mozilla.org/en-US/docs/Web/CSS/flex-direction](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-direction)
    2. Within the ".panel > *" selector, add the following code: 

        ```jsx
        flex: 1 0 auto; 
        display: flex; 
        justify-content: center; 
        align-items: center; 
        ```

        - *Explanation:* All of this code has been gone over in previous sections. But to briefly explain, we are firstly "telling" the text how much space they should allocate for themselves in the panel container.
3. All the elements are now set up statically. In this section, we will incorporate JavaScript and more CSS to animate the panels. 
    - Brief overview of "animations" with CSS and JavaScript: To accomplish an "animation" in CSS, you want to define "class selectors" for the beginning state of an animation and the end state of an animation and the transition in between (in our project, "transition" is in "panel"). In JavaScript, you handle user input (mouse click, key press, etc.) which then calls JavaScript functions which in our case, simply changes the class attribute of an element which changes corresponds to a CSS class selector which will then change its defined behavior (will make more sense as you go down the steps).
    1. Like described above, we will first create class selectors in CSS which will define the beginning and end state of the text within the panels. 
        1. The animation we want to recreate (gif above) first entails the movement of the text at the top and bottom of each panel. To define their beginning and end states, add the following code below the ".panel > *" class selector. 

        ```jsx
        .panel > *:first-child { transform: translateY(-100%)} 
        .panel.open-active > *:first-child { transform: translateY(0);} 
        .panel > *:last-child { transform: translateY(100%);} 
        .panel.open-active > *:last-child { transform: translateY(0);}
        ```

        - *Explanation* :
            - ".panel > *:first-child { transform: translateY(-100%)}" and ".panel > *:last-child { transform: translateY(100%);}" details the "beginning" state of the top and bottom text on each panel. The transforms essentially push the text off the webpage.
            - ".panel.open-active > *:first-child { transform: translateY(0);} " and ".panel.open-active > *:last-child { transform: translateY(0);}" details the "end" state of the top and bottom text on each panel. The transforms push the text back onto the webpage which should happen on click of the panel, which will be implemented in the javascript section coming up
    2. To listen for user input, we will now implement JavaScript which must be done in the <script> </script> tags all the way at the bottom of the body tag (the script tags should already be there for you). Our animation itself will be split into two parts. "Opening" the image (this CSS was given to you) and then the moving the text (which we defined in CSS together above). We'll first tackle "opening" the image. 
        1. To obtain an array of panel elements, add the following code: 

        ```jsx
        const panels = document.querySelectorAll('.panel'); 
        ```

        2. Now, let's write a simple JS function that will "open" each panel image.  
            -  Name your function toggleOpen(). 
               - Syntax: function functionName() \{*Your Code*\} 
            -  This function will be called on a panel and its goal is to add 'open' to that panel's class name.
            -  Try to write one line of code that accomplishes this. (Hints below) 
                - "this" : a keyword that can reference the object you are calling the function on
                - "classList": a property of an element that holds the DOMTokenList of the element's class attributes. (Simply put, it's a list of all the element's classes) 
                - "toggle(token)": A function which will add the passed in token (essentially a class name) to an element's class list. In this case, our token should be 'open'. Note: Make sure to put quotes around your token. So toggle('open'). 
                - ".": an access modifier. You use this to access properties of whatever object you're calling it on. To access the classList of an element for example, you would write: "this.classList".  
                - Put it all together: this.classList.toggle(token). 
        3. After creating your function, add this line of code below your function:
        ```jsx
        panels.forEach(panel => panel.addEventListener('click', toggleOpen));
        ```

        - *Explanation* : The piece of code is a for each loop which is a loop variant that'll iterate through every single element of the container you called it on. In this case, it's iterating through the array of panel elements we defined earlier and for each panel element, is making it so that when it is clicked, "toggleOpen()" is called on it. 
        - Extra reading:
            - addEventListener: [https://www.w3schools.com/js/js_htmldom_eventlistener.asp](https://www.w3schools.com/js/js_htmldom_eventlistener.asp)
    3. The movement of your images should now be functional. We now are now going to implement the movement of your text. Recall in the CSS section, we defined the CSS selector for 'panel.open-active'. To animate our text, we now want to toggle between a panel's default class ('panel') to ('panel.open-active'). This will be done very similarly to how we animated the "opening" of our images.  
        1. Write a function named "toggleActive(e)" (below toggleOpen(), but above the for each loop). It'll also be called on a panel and its goal is to move the text in the panel upon a "transitionend" event. The parameter 'e' will be a transitionend Event passed into the function. 
            - *Explanation*: We want our text to appear only after a user clicks and opens a panel. Thus our function moving the text should also be called after a panel has been opened. The "transitionend" event is CSS's way of telling us that a panel has been opened. 
            - "transitionend" event: This is just an event that is fired following a CSS transition
            - More info on transitionend events: [https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/transitionend_event](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/transitionend_event)
        2. In toggleActive(e), use the "propertyName" attribute of the transitionend Event (e) to check if the CSS property associated with the transitionend event contains ('flex'). If so, in a very similar fashion to the previous function, add 'open-active' to the panel's class name. (Hints below) 
            - *Explanation*: There are several "transitionend" events that are firing within our CSS script. The one associated with opening one of our panels contains 'flex' in the propertyName. 
            - Unlike the previous section, there's a condition for changing the class name. This probably calls for an "if" statement. 
            - In the if condition, you want to check if e.propertyName includes'flex'. The function .includes(string) is an easy way of accomplishing this.
                 - Reading: [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/includes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/includes)
            - The body of your if statement should change the class name of the panel this function is called on to include 'open-active'. Reference your previous function on how to do this. 
       3. After creating your function, add this line of code below the preexisting for each loop. 
        ```jsx
        panels.forEach(panel => panel.addEventListener('transitionend', toggleActive));
        ```

        - *Explanation:* Much like the previous for each loop, this one is iterating through all the panels we defined earlier and is simply looking to call "toggleActive" when a "transitionend" is perceived by one our panels. 
4. With this, your site should be all functional. Feel free to add different images and text. 
    - Further reading on how to add images:
        - [https://www.w3schools.com/html/html_images.asp](https://www.w3schools.com/html/html_images.asp)
        - [https://www.w3schools.com/css/css3_images.asp](https://www.w3schools.com/css/css3_images.asp)
    - Guide from: [https://courses.wesbos.com/account/access/6068e78675ff3a25a5c43b9e/view/194130264](https://courses.wesbos.com/account/access/6068e78675ff3a25a5c43b9e/view/194130264)
    - Documentation by: TritonHacks- Christopher Yoon
