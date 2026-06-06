# livestock-trading
This is a generic livestock trading skill for Claude (although can be used in other AI tools). It can be used to generate trading scenarios to work out what the most profitable buy or sell is at a particular point in time.

First step is to get some market data, which you can download from the MLA site. I've included some examples in the example-data folder. One day I'll get around to hooking up to the MLA api to get this data directly without having to download.

Drag the MLA CSV files into Claude, and then start with a sell prompt like:

```
These are my local market data files. I have 20 x avg weight 500kg steers to sell. Assuming my cost of carry is $200 /day, what would be the most profitable type of cattle to buy in this market?
```

Then you might want to start exploring buy scenarios

```
I have a separate opportunity to buy 30 PTIC heifers that will calve in spring- are they over or under valued at the moment?
```

**Stuff to change**
Make sure you tweak things like:
- average daily gain for your operation
- what weight you can comfortably take animals up to
- your cost of carry, and how you attribute that (per head, per day, etc) as well as carrying capacity
- this will check auctionsplus for sales data, but you may have other sources as well that you'd like to add in (like an agents report)

**Profit and Loss**

This has a somewhat unusual P&L format, as I needed to evaluate both the "sell buy" methodology profit (where I take the cost of carry and pay that to myself) and actual cash P&L (which is what my accountant and wife are interested in)


