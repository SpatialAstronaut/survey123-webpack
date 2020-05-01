# JavaScript Functions for Survey123
With newer releases of Survey123 you are allowed to extend the function of a question by including a JavaScript file in the extension for the survey.  This provided the survey the ability to leverage additional logic to calculate the value of a question.  This function came with some limitations on how much JavaScript could be included in the question.  This library allows the user to bring in WebPacked JS functions and additional libraries to extend that even further.

## Getting Started
Clone this repository to any location on your machine.  Once downloaded run `npm install` in the directory to install the needed node modules.   The JavaScript functions for Survey123 will get installed in the Extensions directory for the Survey.  
---ADD IMAGE HERE---

## Developing with the Extensions
Most mode modules can be included in the WebPack for the Javascript libraries.  Simply develop the functions in JavaScript as you would any other JS functions.  

Run `npm build` to create the packed version of your extension that can be included in the Extensions directory of your Survey.

This library contains the JavaScript functions used by Survey123 in addtion to polyfills needed to support more advanced functions.  This are included in the `survey123\lib.js` library.

### Including the Extension in your Survey
Once you have developed you library and are ready to include in your Survey.  Make sure to include in the extension directory.  You can create a symbolic link from the webpack out file into the Surveys extension directory.  

To call the function from within the survey you call use a calculation on a field to get data.  
`pulldata("@javascript", "<Name of the Extension file>.js", "<Name of the Extension file>.<name of the function>",<parameters comma seperated list>)`

`pulldata("@javascript", "MyLibrary.js", "MyLibrary.reprojectPoint",${x}, ${y})`
MyLibrary.js is the resulting javascript file stored in the extensions directory
MyLibrary.reprojectPoint this will call the reprojectPoint function inside the index.js file
${x}, ${y} these are parameters that are passed into the function

```
export function reprojectPoint(x, y) {
  try{
    if(isNaN(x) || isNaN(y)){
      return "Please provide numbers for x or y"
    }

    if(!isFinite(x) || !isFinite(y))
    {
      return "Please provide finite numbers for x or y"
    }

    //https://spatialreference.org/ref/epsg/indian-1960-utm-zone-48n/proj4/
    var projectTo = "+proj=utm +zone=48 +a=6377276.345 +b=6356075.41314024 +units=m +no_defs";
    return proj4(projectTo,[x,y]);
  }catch{
    return ""
  }
}
```

### Samples
Please visit the Wiki page for Sample on how to use the functions inside Survey123
- Projecting from WGS84 to another projection (https://github.com/EsriPS/survey123-webpack/wiki/Sample:--Projecting-a-Point-from-WGS84-to-another-projection)
