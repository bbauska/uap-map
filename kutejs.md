<p>The KUTE.js Text Write component enables animation for content Element targets by manipulating 
their string contents.</p>

<p>The component provides two properties:</p>

<ul>
  <li>number: NUMBER - interpolate the string numbers by increasing or decreasing their values</li>
  <li>text: STRING - will add/remove a content string one character at a time</li>
</ul>

<p>This text property comes with an additional tween option called textChars for the scrambling text 
character, with the following expected values:</p>

<ul>
  <li>alpha use lowercase alphabetical characters, the default value</li>
  <li>upper use UPPERCASE alphabetical characters</li>
  <li>numeric use numerical characters</li>
  <li>symbols use symbols such as #, $, %, etc.</li>
  <li>all use all alpha numeric and symbols.</li>
  <li>YOUR CUSTOM STRING use your own custom characters; eg: 'KUTE.JS IS #AWESOME'.</li>
</ul>

<p>It's always best to wrap your target number in a &lt;span id="uniqueID"&gt; for the number property 
and content targets should be split into multiple parts for the text property if the target has 
child contents, as we will see in the examples below.</p>

<h4>Examples</h4>
<p>The effect of these two properties is very popular, so let's go through some quick examples to 
explain the workflow for the best possible outcome. We will try to focus on the text property 
a bit more because we can optimize the content targets for a great visual experience.</p>

<h4>Number Property</h4>
<p>As discussed above, the target number need to be wrapped in a tag of choice with a unique ID 
so we can target it.</p>

```HTML
// the target number is wrapped in a <span> tag with a unique ID
<p class="text-example">Total number of lines: <span id="myNumber">0</span></p>
```
```JavaScript
// sample tween object with "number" property
// this assumes it will start from current number which is different from 1550
var myNumberTween = KUTE.to('#myNumber', {number: 1550}); 
```

<p>The above should work like this:</p>

<p>Total number of lines: 1550</p>

<h4>Start</h4>
<p>The button action will toggle the valuesEnd value for the number property, because tweening a 
number to itself would produce no effect.</p>

<h4>Text Property</h4>
<p>Let's try a quick example and analyze the current outcome. Be aware that the example involves 
using child contents for the values, which is something we need to handle in a special way to 
optimize the visual experience.</p>

```JavaScript
// sample tween object with "text" property
var myTextTween = KUTE.to('selector', {text: 'A text string with <span>child content</span> should do.'});
```

<h4>Start</h4>
<p>So targets with child elements don't animate very well it seems. We could probably split the 
starting content and end content into multiple parts, set a tween object for each parth with 
delays and other settings, but Text Write component comes with a powerful utility you can use 
to ease your work in these instances.</p>

<p>The createTextTweens() utility will do it all for you: split text strings, set tween objects, 
but let's see some sample code:</p>

```JavaScript
// grab the parent of the content segments
var textTarget = document.getElementById('textExample');

// make a reversed array with its child contents
var tweenObjects = KUTE.Util.createTextTweens(
  textTarget,
  'This text has a <a href="index.html">link to homepage</a> inside.',
  options
);

// start whenever you want
tweenObjects.start();
```

Now let's see how we doin:</p>

<h4>Start</h4>
<p>There are some considerations for the createTextTweens(target,newText,options) utility:</p>
<ul>
  <li>The utility will replace the target content with &lt;span&gt; parts or the children's 
    tagNames, then for the newText content will create similar parts but empty. Also the number 
	of the parts of the target content doesn't have to be the same as for the new content.</li>
  <li>The utility returns an Array of tween objects, which is similar but independent from tweenCollection objects.
  <li>The returned Array itself has a start() "method" you can call on all the tweens inside.
  <li>The utility will assign playing: boolean property to the target content Element to prevent 
unwanted animation interruptions or glitches.
  <li>While you can set various tween options like easing, duration or the component specific textChars 
option, the delay option is handled by the utility automatically.
  <li>The utility has a special handling for the duration tween option. You can either set a fixed 
duration like 1000, which isn't recommended, or auto which will allow the utility the ability 
to determine a duration for each text part
  <li>When the animation of the last text part is complete, the target content Element.innerHTML will 
be set to the original un-split newText.
  <li>Using yoyo tween option is not recommended because it doesn't produce a desirable effect.
  <li>The utility will not work properly with targets that have a deeper structure than level 1, which 
means that for instance <span>Text <span>sample</span></span> may not be processed properly.</li>
</ul>

<p>Combining Both</p>
<p>0</p>

<p>Clicks so far</p>

<h4>Start</h4>
<p>In this example we've used the textChars option with symbols and all values respectively, but 
combining the two text properties and some other KUTE.js features can really spice up some 
content. Have fun!</p>

<h4>Notes</h4>
<p>Keep in mind that the yoyo tween option will NOT un-write / delete the string character by 
character for the text property, but will write the previous text instead.</p>

<p>For a full code review, check out the ./assets/js/textWrite.js example source code.
The component is only included in the demo/src/kute-extra.js file.</p>


