# How browser works

## What a browser actually is?
A browser is a translating application software that translates markup languages like html, xml etc., style sheets, javascripts, media graphics such as GIFs, images into a user accessible interface that is easy to understand and interact for the user. A browser generally needs internet connection to work and shows information through the web using domains that are changed to a particular IP address.

### The Browser components
![alt text](https://www.researchgate.net/profile/Samuel_Agaga2/publication/322398374/figure/fig1/AS:581617351049216@1515679790727/Web-Browser-Components-2.png "components")

The browser contains the following components -
* User Interface
* Browser Engine
* Rendering Engine
* Networking
* Javascript Interpretor
* UI backend
* Data persistence

**1. User Interface -** Everything that can be seen by the user but cannot be manipulated / modified. Example is the address bar, the user can change the text in the address bar but cannot change the look of the address bar.

**2. Browser Engine -** It is the mediater between User Interface and the rendering engine. Example, if a user presses the button to refresh the page then the browser engine is actually responsible to refresh that page.

**3. Rendering Engine -** It is responsible for parsing HTML, CSS, JavaScript and based on the result of parsing all these, it renders on the browser.

**4. Networking -** Networking uses network layer. The network layer loads the resources of the current page. All the data that is shown on the browser is fetched from the server, hence network layer is responsible to fetch these data through certain protocols.

**5. JS Interpretor -** It basically interprets the javaScript code that is available for the current page.

**6. UI backend -** It is designed to develop common widgets.

**7. Data persistence -** The browser may need to store some data locally, hence local storage, cookies, index db, file system etc. is provided by the persistance layer.

## Sending and Receiving information
![alt text](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2018/09/computer-receiving-data.png?w=730&ssl=1)

The data actually is sent or recieved by the browser through a stream of packets sized in bytes. So in order to render the actual page, the browser needs to convert these bytes into characters. So this is the first step of the browser.

![alt text](https://i0.wp.com/blog.logrocket.com/wp-content/uploads/2018/09/bytes-converted-characters.png?w=730&ssl=1)

## Flow of basic rendering through Rendering Engine
**Parsing --> DOM --> CSSOM --> Render tree --> Layout --> Paint** 

1. ### Parsing -
Parsing is translating of documents into a structure that the code can use. It contains a parser that has a language. The language of the parse has its own grammar consisting vocabulary and syntax rules. The parsing stage consists of the followig steps :-

* Lexical analysis - This step is also called as tokenization and converts the characters into the tokens. The parser converts each line of code into the tokens, such as for a ``` <p> ``` paragraph tag the lexical analyzer will create a different token than for an ``` <a> ``` anchor tag. This is so to create different rules for different tokens. Conceptually, tokens are a data structures containing information about a particular tag in the HTML document.

* Syntax analysis - This works closely to lexical analysis and is responsible to check for legal tokens. Only those tokens are parsed which follow the syntactical rule present in the syntax analyzer.

2. ### Document Object Model -
After the parsing that converts the characters into tokens, these tokens are then converted into the nodes. Nodes are the distinct objects with specific properties. They can be easily understood as the seperate entities of the document object tree. These nodes are put in a tree datastructure hence are the elements of the document object model tree.

![alt text](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2018/09/dom-example-graphic.png?w=730&ssl=1)

Depending on the size of the HTML file the DOM will take some time to be built by the browser. This DOM contains logical nodes.  

3. ### CSS Object Model -
A stylesheet is typically linked to the HTML file as shown below :-
```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" type="text/css" href="index.css" />
</head>
<body>
    
</body>
</html>
```
As the browser recieves the bytes for the HTML for creation of DOM, it also fetches the stylesheet. While parsing it finds the link tag for the stylesheet it also fetches that. It recieves the bytes of data for the CSS. Like the DOM, CSSOM is also created for the CSS in similar fashion.

![alt text](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2018/09/css-bytes-converted-cssom.png?w=730&ssl=1)

The [cascade](https://blog.logrocket.com/how-css-works-understanding-the-cascade-d181cd89a4d8) in the CSS is used to determine what styles need to be applied to an elements.

4. ### Render Tree -
Now we have got two trees, DOM and CSSOM. These are two independent structures. The DOM contains information about all the element's relationships while the CSSOM contains all the information about what styles need to be applied to the elements.
The browser engine then converts DOM tree and CSSOM tree into one. This new tree is known as Render tree. Now the Render tree will have all the information about the element relationships along with the styles for each and every element.

5. ### Layout -
After the construction of the Render tree, the elements need to be placed on the viewport of our browser. This is done by the Layout which calculates all the information about where and how the elements need to be placed on the viewport of the browser. This step is also know as 'Reflow'.

6. ### Paint -
Now as all the necessary details about the elements such as from DOM and CSSOM, i.e, Render Tree, and also the positions from the layout have been calculated, the elements can be now rendered on the viewport of the browser. This step is called as Paint which renders all the elements on the browser.

## JavaScript loading -
So we have understood how the basic HTML and CSS files will be rendered on the browser. But what about javaScript files? Well the answer is that whenever a script tag is encountered during the parsing for the DOM. The further operation is halted / paused until the whole script is loaded. This is because javaScript can manipulate both DOM and CSSOM i.e, both the HTML and the CSS of our code. 

Hence linking a javaScript is a concern for the performance. Let us take an example, if our javaScript is locally present, them it will take just some milliseconds to load. But what if we need to get this file using the API and the network is very slow, this will then halt the entire process of DOM as well as CSSOM and hence the user has to look at a blank screen that is taking too long to load. Hence being a concern to the performance of the page.

Usually it is seen that the script tags are provided at the end of the body.

```html
 <!DOCTYPE html>
<html>

<head>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>Demo</title>
    <link rel="stylesheet" href="style.css">
    
</head>

<body>
    <p id="header">How Browser Rendering Works</p>
    <div><img src="https://i.imgur.com/jDq3k3r.jpg">
      
    <script src="https://index.js"></script>
</body>

</html>
```

This is so that almost all the HTML file is parsed before encountering the script tag. One of the solutions for boosting the performance is to use the script as async. 
```html
 <!DOCTYPE html>
<html>

<head>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>Demo</title>
    <link rel="stylesheet" href="style.css">
    <script src="https://index.js" async></script>
</head>

<body>
    <p id="header">How Browser Rendering Works</p>
    <div><img src="https://i.imgur.com/jDq3k3r.jpg">
</body>

</html>
```
The asynchronous loading of the script means that the parsing will not halt and will go on even if the script is not fully loaded yet.

The entire process of this path of rendering HTML, CSS and JavaScript in a way is known as _**Critical Rendering Path (CRC)**_. Optimizing of this CRC is most important in increasing the performance of rendering of a page.

## Conslusion
So we have understood the basics of how the browser actually renders HTML, CSS and JavaScript.
