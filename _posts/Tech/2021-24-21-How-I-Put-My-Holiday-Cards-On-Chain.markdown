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

<h1>How I Made My Holiday Cards NFTs, Kind Of —&nbsp;And How You Can Get In On The Next Block</h1>

<p><em>Before we begin, I want to make it clear that this isn’t a ‘genuine’ NFT, in many regards and for a few reasons. If anything, this is more of a traditional token (and it’s not even that) since the physical holiday cards themselves all have the same properties — they’re literally fungible. </em></p>

<p><em>There is no money being made here. In fact, the proof of work this cost me along with the total lack of utility means I’ll forever be in the red. But, this is an interesting project &amp; one that gave me a much deeper understanding of blockchain. I hope it does the same for you.</em></p>

<p>In this post I’m going to go over</p>

<ol>
	<li>Why I did this —&nbsp;and a brief history lesson </li>
	<li>How I did it —&nbsp;and why this works</li>
</ol>

<p></p>

<ol start="3">
	<li>How to confirm your card if you’re a node. i.e., got my card in the mail</li>
</ol>

<h2>Project Overview &amp; Inspiration</h2>

<p>First, I’ll address your initial questions: &quot;what is this thing, Nick? I thought blockchains were on the web, or in the cloud, or.. everywhere! Not on a Christmas card!&quot;</p>

<p>You’d be mostly correct, but not fully.</p>

<p>I’ve been involved in crypto since 2017 and the past year have been working as the Growth Manager at <a href="https://mimo.capital/">Mimo</a>, a stable coin issuance protocol.</p>

<p>Earlier this year I started watching Gary Gensler’s MIT course on blockchain and money. While my mind was consistently blown, a particular section caught my attention. Gary asked the class what the longest running blockchain is. Naturally, murmurs of Bitcoin fill the air. But although Bitcoin solved somethings in a novel fashion and created a new application for the tech, the first blockchain was consistently running for more than a decade prior.</p>

<p>As it turns out, this blockchain has been maintained since 1995 and a new ‘block’ is published every Sunday in the New York Times. </p>

<figure><img src="/img/nyt-hash.jpg"/></figure>

<p>Now, bear with me, because if you don’t understand crypto entirely yet, this will truly make things seen in a different, tangible light. </p>

<p>First, we need to understand some quick fundamentals on blockchain.</p>

<p>We’ve got a couple key components:</p>

<p>Hashes, nonces, and unique, immutable data.</p>

<p>When it comes to cryptocurrencies you’re probably using today, the <em>data</em> piece are the transactions that take place in each block; while the hashes and nonces might even go unseen as we abstract ourselves away from the underlying tech.</p>

<p>So, when you consider that this is all that is required to successfully maintain a blockchain, you realize tech itself, or computers, aren’t even necessary - though, makes it actually feasible, sure.</p>

<p>The New York Times is a pretty popular publication that gets distributed into many hands. These act as our confirmation nodes. The Times is also dated. Something that couldn’t really be forged. </p>

<p>Vitalik Buterin even mentions how <a href="https://twitter.com/VitalikButerin/status/1034014732984967175">difficult it would be to break this chain</a>. The same attack could be done if someone decided to forge my greeting cards.</p>

<h2>How &amp; Why This Works (through a distributed greeting card)</h2>

<p>Enough history, here’s where we are now. Although I would’ve loved to build a chain from scratch, the tech debt became entirely unworthy. It’d not be unlike spending $100 in electricity to mine $0.50 of ETH. So, take some liberties with me and enjoy the ride no matter how unsecure it is.</p>

<p>I piggyback off <a href="https://andersbrownworth.com/blockchain">Anders Brownworth’s Blockchain 101 site</a>. </p>

<p>Now, of course we shouldn’t trust anyone, so I decided — in typical crypto fashion — to fork it. Even more on-brand, we hard forked it. This just means that I’m now maintaining this separately from the upcoming blocks on the <em>other</em> chain. Of course in this example, it’s not a live, moving, chain, but the concepts are still there.</p>

<p>Although you can install your own instance of the <a href="https://github.com/NickCalabs/blockchain-demo">Nick Calabro Holiday Card Global Chain</a>, Anders’ works as of right now so you can simply use this.</p>

<p><strong>So, how’s it work?</strong></p>

<p>First let’s look at the components of the holiday card. We have the <code>.png</code>, a hash, a URL, and - most importantly - a postage date. The date is what makes each and every block unique &amp; verifiable.</p>

<p>For the purposes of my cards, we assume their dates are Christmas day of that block’s year. Again, this is a brutally imperfect implementation.</p>

<p>The schema for how we display the block’s data is as follows:</p>

<p>December 25, 2021</p>

<p><URL></p>

<p>If we imagine that date having to match an immutable postage date, we have a truly verifiable blockchain.</p>

<p>The URL is also a piece that an attacker would be needing a forge as well.</p>

<p>So, all we have to do is mine the first block, enter the data from the card onto a new block, and mine #2. If the created hash matches the one on the token, you’ve verified your holiday card is truly from me.</p>

<p>Of course the vector for attack is ripe. There’s only 1 block so far. So, in theory, all one would have to do is send everyone cards with my face on it, make up their own data, and they can effectively hack the chain.</p>

<p>There are 2 reasons why it’s still entirely easy:</p>

<p>As the years go on and I send more cards, the chain grows, and then the attacker would have to repeat the previous year’s cards to recreate the entire chain to match up with their current block’s hash.</p>

<p>()</p>

<p>As the years progress, you’ll have to install the chain’s history to catch up. And, as more years go on, more people are going to enter my network and start receiving holiday cards. These are all extra entities carrying a copy of the ledger since they’ll have that latest hash.</p>

<p>If almost everyone in my network decided to burn their cards, and an attacker tried overtaking the whole chain, so long as there’s 1 person with a card still on their fridge, that would disprove the new side-chain. </p>

<p></p>

<h2>How To Confirm Your Holiday Card On-Chain</h2>

<p>To ensure you’re getting genuine holiday cards from me, the one and only, you can either trust me, or you can verify me.</p>

<p>Why would you trust the greeting card is from myself &amp; truly authentic? Well, the incentive to fake a holiday card from me is quite low. These cards aren’t being traded and they’re really an ephemeral slice of cardboard that gets tossed by 95% of recipients anyway. Trust, in this case, is not a bug - nor even a feature.</p>

<p>But, in the spirit of overly-optimizing, let’s verify:</p>

<h3>Details &amp; Schema</h3>

<p>To collect the data necessary to confirm the hash on your card, there are a couple things we’ll need to look out for.</p>

<p><strong>Date</strong>: an immutable timestamp that can never be recreated.</p>

<p><strong>Unique id</strong>: each block (holiday card) comes with an image. That URL is used to verify as well.</p>

<p><strong>Genesis Block</strong>: The first block has already been mined and is ready to be built upon, the data in that block also helps make up the hash that you’re seeing on your card now. If you can’t confirm that first block, you’re already side-chaining and no further cards can be trusted.</p>

<p>You’ll know you have the correct Genesis Block when:</p>

<ul>
	<li>its previous hash is 64 0s</li>
	<li>its data is simply: <code>nick.finance</code></li>
	<li>its nonce is: <code>4008</code></li>
	<li>and its current hash after mining is <code>000094ad55d3f7eccb5a1c1d6f1d2c36f52d77ee47ca98a26371267cddb53bf0</code></li>
</ul>

<h3>Step 1: Claiming</h3>

<p>One of my investment philosophies is to buy things that you <em>want to own</em>. This is how most die-hards get away with holding on to certain NFTs regardless of where they’re trading at. By accepting the card, you’ve &quot;claimed your token&quot;.</p>

<h3>Step 2: The Timestamp</h3>

<p>One of the properties surrounding this entire chain is the concept of time. Since there’s only 1 moment of time that can never be changed, it’s a great function to use as it’ll be different every <em>time</em>.</p>

<p>Now, if we were creating a truly secure chain, we’d already be broken, as the postal service is subject to human-error and may not stamp the card at the chain’s expected time. So for the purposes of the Nick Calabro Holiday Card Global Chain, we’re going to assume the date is <em>always</em> December 25.</p>

<h3>Step 3: The Data</h3>

<p>Now that we have all the components necessary to mine, we can go ahead and put it together to create the next block’s hash that is written on your card.</p>

<pre><code>December 25, 2021
https://nick.finance/holiday-2021.png</code></pre>

<p>This is block #2’s data. If we followed this patter, you could even guess next year’s, but that would be wildly insecure! So, know that although we’re not running this through any random number generators, the filename for the next block will not be <code>holiday-2022.png</code>.</p>

<h3>Step 4: Mining</h3>

<p>As we discussed above, this is a hard fork from Anders Brownworth’s example that you can find <a href="https://andersbrownworth.com/blockchain/blockchain">on his website</a>. In fact, you might be best off using his to confirm (for now) since you’ll be forced to install and run my fork locally. This aspect is even similar to the first days of Bitcoin.</p>

<p><em><strong>Edit</strong>: As of writing this, the <a href="https://github.com/NickCalabs/blockchain-demo">Nick Calabro Holiday Card Global Chain</a>hard fork is not modified for prime-time. Tech debt..</em></p>

<p>So, Whether you’re using Anders’ or installed your own ledger locally, let’s mine the chain and confirm you have an authentic greeting card.</p>

<p>On the blockchain tab, you’ll find a series of blocks.</p>

<figure><img src="/img/block-1-unmined.png"/></figure>

<p>We know the data for the genesis block: <code>nick.finance</code>, and that’s all you need to input before hitting <code>mine</code>.</p>

<p>Mining the block will find the correct nonce to create our first hash. On the second block, gather your data and enter it. The previous block’s hash is pre-loaded for you.</p>

<figure><img src="/img/block-2-unmined.png"/></figure>

<p>You’ll notice the block is still red, meaning it hasn’t been mined yet or simply broken. If the data is entered correctly, go ahead and mine the block.</p>

<p>Finally, block #2’s hash should match the card on your greeting card, thus giving you proper verification that this is truly from me. </p>

<p>You’ll know you won if your screen looks like this:</p>

<figure><img src="/img/confirmed-chain.png"/></figure>

<h3>Known attack vectors &amp; imperfections:</h3>

<ul>
	<li>The timestamp. The postal service is run by humans. Humans are prone to error. Unlike the NYT example where the operation is smooth enough to ensure there’s always an issue being distributed on a particular day of the week, I don’t trust that we can be so specific with this case.<br/><br/>So, by creating the rule now, that the date of new blocks will be December 25, we can assume this will always be true —&nbsp;until we hard-fork and change the whole thing again. </li>
</ul>