# Exploring-Cryptocurrency-Market

![Forks](https://img.shields.io/github/forks/shukkkur/Exploring-Cryptocurrency-Market.svg)
![Stars](https://img.shields.io/github/stars/shukkkur/Exploring-Cryptocurrency-Market.svg)
![Watchers](https://img.shields.io/github/watchers/shukkkur/Exploring-Cryptocurrency-Market.svg)
![Last Commit](https://img.shields.io/github/last-commit/shukkkur/Exploring-Cryptocurrency-Market.svg) 

To better understand the growth and impact of Bitcoin and other cryptocurrencies I explore the market capitalization of different cryptocurrencies.

<h3>1. Bitcoin and Cryptocurrencies</h3>

<p>Since the <a href="https://newfronttest.bitcoin.com/bitcoin.pdf">launch of Bitcoin in 2008</a>, hundreds of similar projects based on the blockchain technology have emerged. We call these cryptocurrencies (also coins or cryptos in the Internet slang). Some are extremely valuable nowadays, and others may have the potential to become extremely valuable in the future<sup>1</sup>. In fact, on the 6th of December of 2017, Bitcoin has a <a href="https://en.wikipedia.org/wiki/Market_capitalization">market capitalization</a> above $200 billion. </p>


```python
# Reading datasets/coinmarketcap_06122017.csv into pandas
dec6 = pd.read_csv('datasets/coinmarketcap_06122017.csv')

# Selecting the 'id' and the 'market_cap_usd' columns
market_cap_raw = dec6[['id', 'market_cap_usd']]

# Filtering out rows without a market capitalization
cap = market_cap_raw.query('market_cap_usd > 0')

# Counting the number of values
print(market_cap_raw.count())
```

|       id       | 1031 |
|:--------------:|:----:|
| market_cap_usd | 1031 |
|  dtype: int64  |      | 


<h3>2. How big is Bitcoin compared with the rest of the cryptocurrencies?</h3>
<p>At the time of writing, Bitcoin is under serious competition from other projects, but it is still dominant in market capitalization. Let's plot the market capitalization for the top 10 coins as a barplot to better visualize this.</p>

```python
# Selecting the first 10 rows and setting the index
cap10 = cap[:10].set_index('id')

# Calculating market_cap_perc
cap10 = cap10.assign(market_cap_perc=lambda x: x.market_cap_usd/cap.market_cap_usd.sum()*100)

# Plotting market_cap_usd as before but adding the colors and scaling the y-axis  
ax = cap10.market_cap_usd.plot.bar(title=TOP_CAP_TITLE, logy=True, color = COLORS)

plt.show()
```

<p align="center">
  <img src="https://github.com/shukkkur/Exploring-Cryptocurrency-Market/blob/de64e63f6408a51dfe66617b3d89b033df75b48b/datasets/img2.jpg">
</p>

<h3>3. Volatility in cryptocurrencies </h3>

<p>The cryptocurrencies market has been spectacularly volatile since the first exchange opened.Let's explore this volatility a bit more! We will begin by selecting and plotting the 24 hours and 7 days percentage change, which we already have available.</p>

```python

# Selecting the id, percent_change_24h and percent_change_7d columns
volatility = dec6[['id', 'percent_change_24h', 'percent_change_7d']]

# Setting the index to 'id' and dropping all NaN rows
volatility = volatility.set_index('id').dropna()

# Sorting the DataFrame by percent_change_24h in ascending order
volatility = volatility.sort_values('percent_change_24h')

# Checking the first few rows
volatility.head()
```

|               | percent_change_24h | percent_change_7d |
|:-------------:|:------------------:|:-----------------:|
|       id      |                    |                   |
|   flappycoin  | -95.85             | -96.61            |
| credence-coin | -94.22             | -95.31            |
|   coupecoin   | -93.93             | -61.24            |
|    tyrocoin   | -79.02             | -87.43            |
|  petrodollar  | -76.55             | 542.96            |


<h3>4. We can see that things are a bit crazy </h3>
<p>It seems you can lose a lot of money quickly on cryptocurrencies. Let's plot the top 10 biggest gainers and top 10 losers in market capitalization.</p>

```python
ax = volatility.percent_change_24h[:10].plot.bar(color="darkred", ax=axes[0])
ax = volatility.percent_change_24h[-10:].plot.bar(color="darkblue", ax=axes[1])
``` 
  
<p align="center">
  <img src="https://github.com/shukkkur/Exploring-Cryptocurrency-Market/blob/652a1b17adf3399043984d5f38c94c71700b5e96/datasets/img3.jpg">
</p>
  
  
