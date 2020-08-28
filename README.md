# Deferring CSS for Neos CMS

This *very simple* package provides a way to async-load CSS files.
You can split your CSS into single files to speed up your website (i.e. load css that is most probably below the fold) 
or only required if the feature is present. 

It provides a "DeferCss" AFX-atom with preload hack as described here:  
https://www.filamentgroup.com/lab/load-css-simpler/

## Usage
Add your stylesheet simply by using the atom `TechDivision.DeferCss:Atom.DeferCss`:
```
prototype(My.Package:Page) {  
    head.stylesheets.myAwesomeStylesheet < TechDivision.DeferCss:Atom.DeferCss {  
        href.path = 'resource://My.Package/Public/Css/MyAwesomeStyleSheet.css'  
    }
}
```
You could also add a feature-toggle in case you only want to load a certain stylesheet when the feature is present.
Now you can split your css into clean modules that can only be loaded.  

```
prototype(My.Package:Page) {  
    head.stylesheets.myAwesomeStylesheet < TechDivision.DeferCss:Atom.DeferCss {  
        href.path = 'resource://My.Package/Public/Css/MyAwesomeStyleSheet.css'  
        @if.awesomeFeatureIsPresent = ${q(node).children('main').find("[instanceof My.Package:MyAwesomeNodeType]")}
    }
}
```

Or you can add a ContentCollection to your page where you put 

## Result
This is how your stylesheet is going to be included:  
```css
<link href="/_Resources/Static/My.Package/Css/MyAwesomeStyleSheet.css" rel="preload" type="text/css" as="style" />
<link href="/_Resources/Static/My.Package/Css/MyAwesomeStyleSheet.css" rel="stylesheet" media="print" onload="this.media='all';" />
<noscript>
    <link href="/_Resources/Static/My.Package/Css/MyAwesomeStyleSheet.css" rel="stylesheet" media="all" />
</noscript>

```
**Explanation**  
The first link/preload-tag uses modern browser preload functionality so as to prioritize loading.   
The second link-tag is the actual stylesheet that loads as "print", but as soon as it has loaded successfully, it will be loaded as stylesheet.  
The third, noscript-tag is a fallback (if js should be disabled). 


### Sync Stylesheet Atom
Just for feature-completeness, we added also a stylesheet tag `TechDivision.DeferCss:Atom.StylesheetTag` which is of similar usage:
```
prototype(My.Package:Page) {  
    head.stylesheets.myAwesomeStylesheet < TechDivision.DeferCss:Atom.StylesheetTag {  
        href.path = 'resource://My.Package/Public/Css/MyAwesomeStyleSheet.css'  
    }
}
```  
which outputs
```css
<link href="/_Resources/Static/My.Package/Css/MyAwesomeStyleSheet.css" rel="stylesheet" media="all" />
```


## Contribute  
We will be happy to receive pull requests!