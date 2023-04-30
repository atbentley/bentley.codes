+++
title = "Foundations of a space-based economy"
slug = "foundations-of-a-space-based-economy"
description = "This devlog kicks off the journey to build project Stellar Cortex, a space based simulation game. Explore how the foundational systems of information loops and terminals connect pilots across vast distances to enable an effective economy in space."
date = 2023-04-30
authors = ["bentley"]
[extra]
entry = 1
source = "/content/stellar-cortex/001-foundations-of-a-space-based-economy.md"
+++

Welcome to the the first devlog for project stellar cortex! Project  stellar cortex is the working title for my space simulation and sandbox game. In this devlog we will explore the basic premise of the game, it's tech stack, and the first foundational features of the game: the information loop and the trade, market, wallet, and broker terminals.

### The premise

In Stellar Cortex there are no countries, no companies, no associations, no organisations. There is only the pilot. The pilot is the economic unit of space, and the pilot fills all the niches in the economy: trader, merchant, industrialist, financer, broker.

The player must start from nothing with only a single ship their name. They must find opportunities to build trust and create wealth. Eventually building up to a network of connected, autonomous vessels that exchange information, extract raw resources, process resources, manufacture goods and trade on behalf of the player.

If the player chooses, they can find themselves as the most trustful pilot in the solar system. The trust opens doors to opportunities to broker the exchnage of goods on markets for other pilots, or to provide escrow services or other financial servies to pilots.

The player may also choose to walk away from the path of trustfulness, tainting their reputation and blocking themselves from future legitimate interactions and instead taking the path of espionage, hacking, and sabotage.

### Tech stack

The game is being written in rust, delegating most of the heavy lifting to the [bevy](https://bevyengine.org) game engine and it's ecosystem of community crates.

### Philosophy

Alongside the tech stack, the philosophy, or approach to building something can be just as influential on the final product.

* No special roles: anything an NPC can do the player can do, and anything the player can do the NPC can do.
* Emergant gameplay
* Extensible
* Fun then beautiful

### Information loops and terminals

Gameplay embraces the vastness of space. Travelling is a comittment and takes time. But moreso, information takes time to propegate as a wave through space. Every command or message must travel through an information network of hardwired connections, radio transmitters and radio receivers until it reaches it's ultimate recipiant. Each pilot has their own view on the world based on the information they have accumulated. This information can be passively collected as they travel through space, it may be bought on a market, it may be acquired from the wrecks of other vessels, and most importantly; it may be wrong.

The system that manages this information is called the information loop. Each pilot has an information loop and each vessel has an information loop. When a pilot enters a vessel, their information loop merges with the information loop of the vessel, and when the vessel docks with another vessel the same merging occurs.

Terminals are the interfaces to the underlying information contained in the information loop. They provide specialised views into the information, as well as providing mechnisms to interact with other terminals on the information loop.

Let's get a little bit more concrete with an example.

{{ stellar_cortex_demo(id="001", size="6") }}

### Selling and buying items from a market in space

The first nugget of gameplay that I want to enable is for one pilot to list a ship for sale on a market, and for another pilot to purchase that ship. This interaction can involve up to four pilots (recalling the 'no special roles' principal):

1. The buyer
2. The seller
3. The market
4. The broker

I think the broker is where things get interesting. In our fictional world there exists a flourishing space-based economy spread across the solar system. In this economy there are many local markets where goods can be traded for fiat currency. The size of the solar system makes it impractical to transport many of the goods vast distances to central markets. This creates a demand for local markets that are close to mining sites and industrial centres. In order for the markets to operate efficiently there also needs to exist a locality of fiat currency in which the market can trade in. This is where the role of the broker comes in. Each broker mints their own currency and is responsible for maintaining a ledger of all transactions occuring in their currency. A broker operating close to a market is attractive as trades can be settled quicker, and pilots can receive their newly purchased goods quicker.
