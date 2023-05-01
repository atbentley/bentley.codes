+++
title = "Foundations of a space-based economy"
slug = "foundations-of-a-space-based-economy"
description = "The first devlog for project Stellar Cortex, a space-based simulation game written in Rust and Bevy"
date = 2023-05-01
authors = ["bentley"]
[extra]
entry = 1
source = "/content/stellar-cortex/001-foundations-of-a-space-based-economy.md"
+++

Welcome to the first devlog for project Stellar Cortex! Stellar Cortex is the working title for the space simulation game that I am working on. This devlog kicks off the journey and explores how some of the foundational systems of *information loops* and *terminals* come together to enable space based commerce.

The game is being written in [Rust](https://www.rust-lang.org/) using the [Bevy](https://bevyengine.org/) game engine. It is a solo, hobby project of mine where the motivation is to build a game that I find interesting to play, as well as to develop. In developing the game I hope to explore and learn many topics related to orbital mechanics, physics, economics, industrial processes and manufacturing.

<!-- more -->

### Premise

In Stellar Cortex, the pilot is the economic unit. The pilot extracts raw resources, refines resources, manufactures goods, transports goods between industrial centres and markets, operates markets, and offers financial services. In the beginning, the player will find themself at the low end of the spectrum of capital. Overtime they will be able to purchase autonomous vessels, expand their operations and enter new markets. Eventually the player will own a supply chain that spans across the solar system, they will operate space stations, own markets, mint their own currency, and construct megaprojects in space.

There are so many features to explore in the future, but for now there needs to be a solid foundation to make all of this possible. Information will play a huge part in Stellar Cortex, so it is a natural starting point to build out.

### Information loops and terminals

Stellar Cortex embraces the vastness of space. Information will need to propagate as a radio wave across vast distances. It will get stale, it will get lost, and sometimes it will be plain wrong. Every pilot in the solar system will have a unique perspective based on the information they have at hand. The *information loop* is the system that takes care of collecting information on behalf of the pilot. When a pilot enters a ship, their information loop and the ship's information loop merge. When a ship docks in a station, the information loop of the ship and the station also merge. Merged information loops can pass messages directly to each other without having to use radio transmitters and radio receivers.

The information loop by itself doesn't know how to process information, that is the role of a *terminal*. Terminals provide specialised views into the data contained within the information loop, they can also use the information loop to send messages to other pilots and terminals.

Let's get more concrete with an example.

### Buying a ship from a market

Let's suppose a pilot buys a new ship. There can be up to four pilots involved in this exchange. The seller, the buyer, the market, and the broker. This exchange can take place over a large distance, with each message sent needing time to travel.

The process to purchase the ship looks like this:

1. First, the buyer requests the current sell orders from the market
2. Noticing that there is a ship for sale of a reasonable price, the buyer decides to purchase it. They send a message to the broker asking for them to authorise the transfer of funds to the market for the purchase price of the ship.
3. The broker receives the authorisation request, verifies that the buyer has enough credits in their account and the sets aside the purchase price, effectively freezing those funds. The broker sends the successful authorisation back to the buyer.
4. The buyer receives the authorisation from the broker. They then send an immediate buy order request to the market. This request contains a reference to the sell order they wish to purchase, as well as the authorisation from the broker indicating they have enough funds to complete the transaction.
5. The market receives the immediate buy order request. If the sell order is still valid, the market takes the authorisation and sends it to the broker, asking the broker to settle it and complete the transfer of funds from the buyer to the market.
6. The broker receives the settlement request and if the authorisation is still valid will complete the transfer, updating their internal ledger so that all accounts are balanced. They send messages to both the buyer and the market indicating the transaction has been completed.
7. The market receives confirmation from the broker that they have the money from the buyer. They then transfer ownership of the ship to the buyer. Afterwards, they create an authorisation request to transfer the sale price minus fees to the seller. This authorisation request is sent to the broker. (The buyer also receives the confirmation that the money was collected by the market, and can update their balance in their own wallet).
8. Similar to step 3, the broker checks the authorisation of funds from the market to the seller is valid and if so sends a response to the market indicating the funds have been authorised.
9. The market receives the successful authorisation from the broker and then sends a message to the seller containing the valid authorization and idicating that the ship has been sold.
10. The seller receives the authorisation and sends a request to the broker to settle the payment.
11. The broker receives the settlement request and updates the ledger if it is valid, sending back a response to the seller indicating the transaction has been completed.
12. The seller receives the successful settlement from the broker, they have their funds and the sell order has been completed.

### Demo

See if you can recreate this process in the demo! The scenario starts off with the sell order already being listed on the market, and the buyer having zero funds. Maybe there is a way to inject some credits into the player's account. In this demo the player, the seller, the market and the broker are all located in the same station, so the messages travel super quickly. But if you look close enough you'll see some flickering as messages come and go and states are changing.

{{ stellar_cortex_demo(id="001", size="6") }}

### State machines and other implementation details

You'll notice along the top left there are the buttons for 'Trade', 'Wallet', 'Market' and 'Broker' depending on which entity you have selected. These are the four terminals that are currently implemented. Every message that enters the information loop will be forwarded to each terminal, the terminal can then decide what to do with it. The terminal may update their local state, they may send a response message back to the information loop, or they may start a *command*.

A command is a small state machine, it is similar to a terminal in that each message that enters the information loop will be forwarded to the command. The command can then decided to send messages back to the information loop and transition its internal state.

In the demo above, when the buyer clicks the buy button an immediate buy command is started. It sends the request to the broker for authorisation and enters a wait state. When the response from the broker arrives, the command knows what to do with it will send a message to the market to initiate the immediate buy.

In the 12 step process above there exists three commands. The sell order from the seller's perspective is a command, the sell order from the market's perspective is another command, and finally the immediate buy order is a command. The broker's interaction is a simple reply / response and doesn't require a more involved state machine, so the Broker terminal can handle this directly without using commands.

### Closing notes

This comes across as a relatively complex process to purchase an in game item. And it is! However, there are two main motivating ideas in building in this way. The first is that it is open to extension. Messages, terminals and commands can be plugged into the information loop, enabling new functionality without having to overhaul the core systems. And secondly, there is a vision here is that this complex process introduces opportunities for novel gameplay, which I would love to explore in future devlogs!

Thanks for reading, if you'd like to follow along there is a [feed](https://bentley.codes/atom.xml). If you'd like to chat, you can find me on the [Bevy discord](https://discord.gg/bevy) as @bentley.
