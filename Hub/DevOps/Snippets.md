# Snippets

## Table of Contents

[About]()
[Page Separator]()
[All Colors]()
[Data Url]()
[View Cookies]()

## About

Snippets.md

Browser Snippets - Google Chrome - [ F12 ] - [ Sources - Snippets ]

Browser Snippets: 

- [ JS ] All Colors

- [ JS ] Data Url

- [ JS ] View Cookies




## All Colors

All Colors 

```js

// allcolors.js
// https://github.com/bgrins/devtools-snippets
// Print out CSS colors used in elements on the page.

(function () {
  // Should include colors from elements that have a border color but have a zero width?
  var includeBorderColorsWithZeroWidth = false;

  var allColors = {};
  var props = ["background-color", "color", "border-top-color", "border-right-color", "border-bottom-color", "border-left-color"];
  var skipColors = {
    "rgb(0, 0, 0)": 1,
    "rgba(0, 0, 0, 0)": 1,
    "rgb(255, 255, 255)": 1
  };

  [].forEach.call(document.querySelectorAll("*"), function (node) {
    var nodeColors = {};
    props.forEach(function (prop) {
      var color = window.getComputedStyle(node, null).getPropertyValue(prop),
        thisIsABorderProperty = (prop.indexOf("border") != -1),
        notBorderZero = thisIsABorderProperty ? window.getComputedStyle(node, null).getPropertyValue(prop.replace("color", "width")) !== "0px" : true,
        colorConditionsMet;

      if (includeBorderColorsWithZeroWidth) {
        colorConditionsMet = color && !skipColors[color];
      } else {
        colorConditionsMet = color && !skipColors[color] && notBorderZero;
      }

      if (colorConditionsMet) {
        if (!allColors[color]) {
          allColors[color] = {
            count: 0,
            nodes: []
          };
        }

        if (!nodeColors[color]) {
          allColors[color].count++;
          allColors[color].nodes.push(node);
        }

        nodeColors[color] = true;
      }
    });
  });

  function rgbTextToRgbArray(rgbText) {
    return rgbText.replace(/\s/g, "").match(/\d+,\d+,\d+/)[0].split(",").map(function(num) {
      return parseInt(num, 10);
    });
  }

  function componentToHex(c) {
    var hex = c.toString(16);
    return hex.length == 1 ? "0" + hex : hex;
  }

  function rgbToHex(rgbArray) {
    var r = rgbArray[0],
      g = rgbArray[1],
      b = rgbArray[2];
    return "#" + componentToHex(r) + componentToHex(g) + componentToHex(b);
  }

  var allColorsSorted = [];
  for (var i in allColors) {
    var rgbArray = rgbTextToRgbArray(i);
    var hexValue = rgbToHex(rgbArray);

    allColorsSorted.push({
      key: i,
      value: allColors[i],
      hexValue: hexValue
    });
  }

  allColorsSorted = allColorsSorted.sort(function (a, b) {
    return b.value.count - a.value.count;
  });

  var nameStyle = "font-weight:normal;";
  var countStyle = "font-weight:bold;";
  function colorStyle(color) {
    return "background:" + color + ";color:" + color + ";border:1px solid #333;";
  };

  console.group("Total colors used in elements on the page: " + window.location.href + " are " + allColorsSorted.length);
  allColorsSorted.forEach(function (c) {
    console.groupCollapsed("%c    %c " + c.key + " " + c.hexValue + " %c(" + c.value.count + " times)",
      colorStyle(c.key), nameStyle, countStyle);
    c.value.nodes.forEach(function (node) {
      console.log(node);
    });
    console.groupEnd();
  });
  console.groupEnd("All colors used in elements on the page");

})();

```


## Data Url

Data Url 

```js

// dataurl.js
// https://github.com/bgrins/devtools-snippets
// Print out data URLs for all images / canvases on the page.

(function() {

  console.group("Data URLs");

  [].forEach.call(document.querySelectorAll("img"), function(i) {
    var c = document.createElement("canvas");
    var ctx = c.getContext("2d");
    c.width = i.width;
    c.height = i.height;

    try {
      ctx.drawImage(i, 0, 0);
      console.log(i, c.toDataURL());
    }
    catch(e) {
      console.log(i, "No Permission - try opening this image in a new tab and running the snippet again?", i.src);
    }
  });

  [].forEach.call(document.querySelectorAll("canvas"), function(c) {
    try {
      console.log(c, c.toDataURL());
    }
    catch(e) {
      console.log(c, "No Permission");
    }
  });

  console.groupEnd("Data URLs");

})();

```


## View Cookies

View Cookies

```js

// viewcookies.js
// https://github.com/bgrins/devtools-snippets
// Shows all cookies stored in document.cookies in a console.table

(function() {
  'use strict';

  window.viewCookies = function() {
    if (document.cookie) {
      const cookies = document.cookie
        .split(/; ?/)
        .map(s => {
          const [ , key, value ] = s.match(/^(.*?)=(.*)$/);
          return {
            key,
            value: decodeURIComponent(value)
          };
        });

      console.table(cookies);
    }
    else {
      console.warn('document.cookie is empty!');
    }
  };
})();

window.viewCookies();

```



## END OF DOCUMENT 

<!-- 
===========================================================================================================
~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~
===========================================================================================================
-->