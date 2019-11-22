---
layout: post
title: How bad is the plastic waste problem?
image: /img/hello_world.jpeg
published: true
---
Wang Jiuliang's independent documentary [**Plastic China**](https://www.youtube.com/watch?v=OJrVYB15aFA) 
(2016, 82 mins), now avaiable on [Youtube](http://www.youtube.com) and [Amazon Prime](https://www.amazon.com/Prime-Video/), [![Plastic China](https://img.youtube.com/vi/OJrVYB15aFA/0.jpg)](https://www.youtube.com/watch?v=OJrVYB15aFA) for the first time reported the depth and width of the problem in plastic waste industry in China. Shortly after it went viral, the documentary waws banned in 2017 from the internet in China. However, the impact of Wang's work in China was said to be comparable to Rachel Carson’s book “Silent Spring” in the Unite States. The environmental movement finally led to the official ban of the import of 24 kinds of solid wastes from foreign countries, implemented on January 1, 2018.

In the documentary, the worker sorting the plastic waste without any personal protection equipment, risking having their skin punctured by discarded medical plastic equipment. Unattended children playing on the plastic waste hill and pick up things to play unknowing that some are tremendously dangerous to their life.

"Plastics are hard to sort. There are hundreds of them, something Polyester, something polychlorine, something benzene, something polyoxymethylene. Experienced worker light them up and can tell by the burning smell," says old worker Yu in the documentary,   
"..., you can find almost any disgusting things among the discarded plastic waste: spoiled meat with fly larvae, ragged dirty socks, stinking dried up spoiled milk. I know this poor 70 year old grandma, who burned her fingers emptying an unlabeled container with Hydrofluoric acid in it. And some are waste metal that contains radioactive materials beyond safe level." 


So, how bad is the situation, how much plastic do we produce per year, where do they go? which sector produces most plastic waste, How are they traded globally?  There are so many questions to be answered and big data to gather. I looked into the data I found from [our world in data](https://ourworldindata.org/) and here are some interesting plot I generated in Python. Hopefully by doing and sharing it, we can look straight at the problem and take actions to deviate from this self-destruction course for human race.   

Based on 2010 data, the total generated plastic waste annually is over 273 million tons, and 1.9 million tons were littered. Given a common plastic credit card of 5 grams, we can picture ourselves generating a median of 5971 credit cards per person per year and each of us littered 55 pieces of the credit cards into other people's land anually.   

It can be seen from the scattered plot below that the Unite States, China, Japan, Brazil, Germany are the top 5 in plastic waste generation. China is the No. 1 in plastic generation. US is the No.1 in plastic litterring and plastic waste per capita. Germany drops out of the top 5 list on littering. India (next to mexico on the scattered plot) are the second most populated country and its small bubble size indicates it has a really low per-capita plastic waste. 
![bubble1](https://github.com/qianjing2020/qianjing2020.github.io/raw/master/plots/plastic_waste/plot_bubble_generation_litter_popu.png)

Gross Domestic Product (GDP) is the monetary value of all finished goods and services made within a country during a specific period. The research question is "Is GDP and mismanaged waste negatively dependent?" The below scattered plot shows the mismanaged plastic waste per capita vs. GDP per capita, buble size being the country population. The data are scattered and there is no visual dependence of the two variables. Waste mismanagement is not necessarily correlated to low GDP, policy, culture, and other important factors all need to be examineded based on each case. Comparing China and India (two largest country populational wise), China obviously can learn from India on how to manage waste better and not stress its environment for GDP growing. The United States seems to do a good job managing its plastic waste while still maintaining the highest GDP, or, its plastic waste was shipped to other countries to avoid high cost of proper domestic manangement?
![bubble2](https://github.com/qianjing2020/qianjing2020.github.io/raw/master/plots/plastic_waste/plot_bubble_waste_GDP.png)

If we look into the per-capita data globally, we can see that Kuwait, Guyana, Germany, Netherland, Ireland, Sri Lanka, and the United States topped the rest of the world on generating plastic waste, as shown below. 
![waste_gen_per_capita](https://github.com/qianjing2020/qianjing2020.github.io/raw/master/plots/plastic_waste/plot_choropleth_waste_generation_per_capita.png)

Since China obviously has the largest plastic waste import in the world before the ban beginig on 2018, a look into its palstic waste source will give insight on the waste traffic globally. The pie chart plots the portion of imports from each countries. The total amount of plastic waste China imported in the year of 2016 is over 7 million tons (7,134,966 tons). Given the population of 1.379 billion in 2016, per capita waste received from foreign country is 5.2 kg. In other words, each person in China were given 1000 wasted credit cards waste to dispose. 
![export_china](https://github.com/qianjing2020/qianjing2020.github.io/raw/master/plots/plastic_waste/plot_pie_export_China.png)

So what can we do, here is a plot showing the sources of the plastic. Plastic generation and waste generation are shown by each sector. As pretty much all environmental engineers and scientists agree, the best way to reduce a waste is to cut it down at the source. From the bar plot, we can tell we are wasting tones of resources, literaly, on packaging! 
![waste source](https://github.com/qianjing2020/qianjing2020.github.io/raw/master/plots/plastic_waste/plot_bar_production_generation.png)

So, I am pondering on what I can do to reduce plastic packaging, and came out with a list:

- Avoid overpackaged gifts
- Bring a canvas tote bag for grocery shopping
- Buy bulk food from local food co-op instead from Amazon
- Considering serve biryani rice in cabage or banana leaves at your party
- Go for [edible spoon/forks](https://www.youtube.com/watch?v=r4Cc5zmy0eY) which are good for earth and tummy 

Okay, enough data story telling for today! I am going back to my kitchen launching a fun homemade edible utensil project. Here is the [recipe](https://www.youtube.com/watch?v=-fr-cBA5P1I) I am going to follow. My next blog will be another story on how the plastic turned into microplastic and today how much plastic each human being is ingesting in the form of microparticle (one credit card per week or possibly more?) Oh, yes, it all comes back at us!  

