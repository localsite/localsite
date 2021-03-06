## Integration between Websites

<a href="https://github.com/modelearth/localsite/" target="_parent">The Localsite Framework</a> is designed to allow easier integration between multiple Github repos by using hash values to share settings.  

**hashChangeEvent** - triggers independent widgets when the URL hash is changed both programmatically and by the user.    

**hiddenhash** - stores values referenced by multiple apps to avoid cluttering the URL.  
<!--
goHash({"go":"bioeconomy"}); - Hash change triggers widgets.  

updateHash({"go":"bioeconomy"}); - Only hash updated.  
-->
**initialParam** - An object containing key-values from three sources: the initial URL hash, parameters in the URL, and parameters set on the javascript include&nbsp;file.  

**loadMarkdown** - Pulls readme files into html and converts links and image tags based on folder levels.  

**lazy class** - Sometimes it's good to be lazy. This class waits to load images until just before they scroll into view.  

