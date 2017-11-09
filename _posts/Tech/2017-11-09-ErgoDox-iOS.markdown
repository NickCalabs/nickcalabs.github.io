---
layout: post
title: "Custom ErgoDox iOS Keyboard"
category: Tech
author: Nick Calabro
excerpt: "We dive into the fundamentals of creating a third-party keyboard for iOS while having some fun making it just like the fan-favorite: ErgoDox."
---

<meta name="twitter:card" content="summary" />
<meta name="twitter:site" content="@NickCalabs" />
<meta name="twitter:title" content="{{ page.title }}" />
<meta name="twitter:description" content="Nick Calabro's Blog" />

<div class="message">The following was typed with a HHKB</div>

<p>The ErgoDox Ergonomic Keyboard is unlike many keyboards you may be used to. Between its durable and customizable build, programmatic ability, and split personality, it truly has so much to offer any and all typists. The most intriguing feature of the keyboard is the layout. Setting aside all the things that make it great, the ErgoDox can still be considered a novel input device with the layout alone. Allowing for extra keys to be hit where they’re most convenient and the ortholinear nature of it makes for one of the greatest typing experiences ever. </p>

<p>Although the unique keyboard relies on human hands resting on its paw-shaped imprint, it’d be fun to have a similar style on our mobile devices. Sure, there are <a href="https://itunes.apple.com/us/app/word-flow-keyboard-english/id1077864246?mt=8">specially shaped keyboard apps for iOS that optimize for your fingers</a>, but none that are simply a fun homage to a mechanical keyboard enthusiast favorite.</p>

<p>When Apple first announced they were allowing third-party keyboards, I was one of the first to hop on board. It was easily my favorite update to iOS that year. People were daunted when they realized the process of building a keyboard was extensive and that there was little provided by Apple to get started. What’s required is essentially building an entire keyboard from scratch, and then whatever special functionality you want on top of that. </p>

<hr />

<h2>Open Xcode</h2>

<p>After you create a new project, head right over to File &gt; New &gt; Target… and choose <em>Custom Keyboard Extension</em>.</p>

<figure><img src="img/targetdox.png"/></figure>

<p>You’ll want to make sure the <em>Product Name</em> is different than the currently encompassing project name. </p>

<figure><img src="img/nametargetdox.png"/></figure>

<p>If you’re asked to active the scheme simply allow it. </p>

<div class="message"><p>Apple defines a target as a <em>product to build</em> that must be contained within an application. This structure is why content blockers, keyboards, and iMessage apps have to be within an app even if it hardly ever gets opened.</p></div>

<p>Now add a Storyboard file using <code>CMD+N</code>, scrolling to <em>User Interface</em> and choosing the Storyboard option. Name it something unique like <em>DoxStoryboard</em>, and create. </p>

<p>Jump to the target’s <code>info.plist</code> file and under <code>NSExtension</code>, remove the <code>NSExtensionPrincipalClass</code> key. Add a new key under <code>NSExtension</code> titled <code>NSExtensionMainStoryboard</code> with the value <code>DoxStoryboard</code> or whatever you named the Storyboard file we created above. </p>

<p>Navigate to your <code>DoxStoryboard.storyboard</code> and drag a <em>View Controller</em> element on the canvas. Since we’re working with storyboards instead of xibs, it may take the shape of an iPhone sized rectangle. With the view controller selected, navigate to the <em>Size Inspector</em>, change the <em>Simulated Size</em> to <em>Freeform</em> and modify the height and width. I have mine at 375x375. In the <em>Attributes Inspector</em> be sure to select <code>Is Initial View Controller</code>. </p>

<figure><img src="img/emptystoryboarddox.png"/></figure>

<p>At this point, we can go ahead and run our application. When navigating to Settings &gt; General &gt; Keyboard &gt; Keyboards &gt; Add New Keyboard… we’ll see our custom board ready to be added. </p>

<figure><img src="img/addnewkeydox.png"/></figure>

<figure><img src="img/selectkeydox.png"/></figure>

<p>To demonstrate more clearly your keyboard in use after enabling it, changing the background color of the storyboard to something garish can assist with providing some instant gratification. Linking the <code>KeyboardViewController</code> class to the view controller will also provide some free functionality with the <em>Next Keyboard</em> button as you can see here.</p>

<figure><img src="img/keydoxinuse.png"/></figure>

<div class="message"><p>Note: I’ve come across some issues while doing something similar with xibs instead of storyboards and had to add the <code>.storyboard</code> file to the Target Membership like so:</p>
<p><img src="img/targetmembdox.png"/></p></div>

<p>Now that we have everything connected, up, and running we can enjoy the fun part. Looking at the <a href="https://www.massdrop.com/configurator/ergodox">ErgoDox Layout</a>, we can see there’s some unique placement, slightly staggering, and many sizes and orientations to consider. </p>

<figure><img src="img/emptdoxlayout.png"/></figure>

<p>If you’ve ever played with views and buttons inside Xcode’s Storyboards you’ll enjoy this because that’s just what needs to be done. </p>

<figure><img src="img/redbuttonsdox.png"/></figure>

<p>Once you’ve monotonously and meticulously placed each key in its proper location, there’s some rotation magic that has to be done in code to get our <em>thumb</em> buttons to act correctly. </p>

<figure><img src="img/clusterviewdox.png"/></figure>

<p>Each thumb cluster should be in it’s own square-shaped view. Here, we’ll call them <code>leftThumbView</code> and <code>rightThumbView</code>. Connect those outlets, be sure these buttons are on top of the view, and configure the view with something along the lines of this:</p>

<pre><code>self.leftThumbView.transform = self.leftThumbView.transform.rotated(by: CGFloat(M_PI_2/2))
</code></pre>

<p>This will get the cluster to rotate as soon as the keyboard loads to give the the effect of the lovely hand-shaped keyboard. </p>

<figure><img src="img/outlineviewdox.png"/></figure>

<div class="message"><p>Just to make the storyboard less jarring, I’ve decided to hew to a certain color scheme. The keyboard community loves creating themed keycap sets and running exclusive <em>group buys</em> so you feel forced to get in on everything before they’re coveted only a few months later. A classic example of this is the <a href="https://originative.co/products/hyperfuse">HyperFuse</a>.</p>

<p>Beautiful and crisp GMK colors made any keyboard look amazing, especially an ErgoDox. In fact — it’s so lovely <a href="https://github.com/NickCalabs/Hyperfuse">I even made an Xcode theme in honor of it</a>. <img src="img/hyperdox.png"/> I figured to hew to the same cool and calm colors for the duration of the keyboard creation.</p></div>

<h2>Typing</h2>

<p>Now that the layout is out of the way we have to get to some actual functionality. There are many ways to go about programming the keyboard and getting the input/output to perform as expected. A popular method is to read whatever the label of the key is and spit that out. Although this is quick and dirty, when we have such a customizable keyboard like this it’s pretty hard to hardcode such generic functionality. We’ll go with this approach for some of the keyboard like so:</p>

<figure><img src="img/completeicondox.png"/></figure>

<p>By having the glyph as the button’s text, we can extract that and have that input with some simple actions:</p>

<pre><code>@IBAction func actuation(sender: UIButton!) {
        let typedChar = sender.titleLabel?.text
        let proxy = textDocumentProxy as UITextDocumentProxy
        proxy.insertText(typedChar!)
}
</code></pre>

<p>Connect this action to most of your keys.</p>

<p>The space bar is much simpler to implement since we only need to insert a nearly empty string &quot; &quot;. Be sure to connect this as well.</p>

<pre><code>@IBAction func spaceActuate() {
        let proxy = textDocumentProxy as UITextDocumentProxy
        proxy.insertText(&quot; &quot;)
}
</code></pre>

<p>Although the power of the ErgoDox is essentially unlimited, the powers of the third-party keyboard tools Apple provides are not unlimited. There are some <em>really</em> interesting things we can do with this, but we’ll cover only the essentials for now and get to deleting and navigating.</p>

<p>Apple has done a swell job of making the basics of keyboard creating fairly simple. The next methods used to delete, hide the keyboard, and navigate the insertion point are self-explanatory:</p>

<pre><code>@IBAction func hideBoard() {
        dismissKeyboard()
}
    
@IBAction func delete() {
        let proxy = textDocumentProxy as UITextDocumentProxy
        proxy.deleteBackward()
}
    
@IBAction func moveRight() {
        let proxy = textDocumentProxy as UITextDocumentProxy
        proxy.adjustTextPosition(byCharacterOffset: 1)
}
    
@IBAction func moveLeft() {
        let proxy = textDocumentProxy as UITextDocumentProxy
        proxy.adjustTextPosition(byCharacterOffset: -1)
}

</code></pre>

<figure><img src="img/erg.gif"/></figure>

<hr />

<h2>The Real Ergo</h2>

<figure><img src="img/mydox.png"/></figure>

<p>Well done. Our ErgoDox for iOS is in a fine place and now it’s time to get to real keyboards. You can view my real <a href="https://github.com/NickCalabs/ErgodoxLayout">programmable ErgoDox layout here</a> and <a href="https://github.com/NickCalabs/ErgoDoxiOS">I’ve uploaded the project here</a>. Programming input peripherals whether virtual or physical is very important.</p>

<p>I have a few ErgoDoxes and glad I was able to bring the novelty of it to mobile devices while learning about Apple’s fun new SDKs. There’re a lot of places this custom keyboard can go and I’m eager to try more things with it. </p>

<p>Be sure to check back on <a href="nickcalabro.com">my site</a> where I <a href="http://nickcalabro.com/TEX-Yoda-Build-Log-and-Review">discuss building keyboards IRL as well</a>. </p>
