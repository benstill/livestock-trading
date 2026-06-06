# livestock-trading
This is a generic livestock trading skill for Claude (although can be used in other AI tools). It can be used to generate trading scenarios to work out what the most profitable buy or sell is at a particular point in time.

First step is to get some market data, which you can download from the MLA site. I've included some examples in the example-data folder. One day I'll get around to hooking up to the MLA api to get this data directly without having to download.

Drag the CSV files into Claude, and then start with a sell prompt like:

```
These are my local market data files. I have 20 x avg weight 500kg steers to sell. Assuming my cost of carry is $200 /day, what would be the most profitable type of cattle to buy in this market?

Then you might want to start exploring buy scenarios

```
I have a separate opportunity to buy 15 PTIC heifers that will calve in spring- are they over or under valued at the moment?