haskell-js-html-builder
=================

A monad and monoid instance for Haste.DOM elements 

Haste is a compiler that generates Javascript code from Haskell.

https://raw.githubusercontent.com/valderman/haste-compiler

The Haste.DOM module define a thin layer over the JavaScript DOM. That makes the creation and manipulation of DOM elements  as painful as in JavaScript.

This package makes the creation of DOM elements easy with a syntax  similar to other haskell HTML generators, using monoids and monads, such is the case of the package blaze-html.

This is an example. `withElem`  is a Haste.DOM call that give the DOM object whose id is "idelem", that has been created "by hand" in Main.hs. The builder takes this element and add content to it:

      main= do
        withElem "idelem" . build $ do
        div $ do
           div  $ do
               p "hello"
               nelem "p" `attr` ("style","color:red")  `child`  "world" 
        return ()

       div cont=  nelem "div" `child`  cont

       p cont = nelem "p"  `child`  cont

The equivalent monoid expression can also be used, by concatenating elements with the operator <>


Status
---------

Still there are no operators defined for attribute addition, so attributes only can be applied to full elements, not to container elements.
For example :
       
    div `attr` ....  content   

will produce an error. 

but:

    div  content  `attr` ....

works


 
How it works
------------


The basic element is a "builder" that has a "hole" parameter and IO action about what element will be created. The hole contains the parent of the element being created. Upon created, it is added to the parent and return itself as parent of the next elements.  to append two elements, both are added to the parent.

The Monad instance is there in order to use the do notation, that add a new level of syntax in the style of the package blaze-html .  This monad invoques the monoid.
