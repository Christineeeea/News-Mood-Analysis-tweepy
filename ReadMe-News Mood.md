

```python
import tweepy
import numpy as np
import pandas as pd
import json
import matplotlib.pyplot as plt
import seaborn as sns
from datetime import datetime
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer = SentimentIntensityAnalyzer()
```


```python
consumer_key = "Ed4RNulN1lp7AbOooHa9STCoU"
consumer_secret = "P7cUJlmJZq0VaCY0Jg7COliwQqzK0qYEyUF9Y0idx4ujb3ZlW5"
access_token = "839621358724198402-dzdOsx2WWHrSuBwyNUiqSEnTivHozAZ"
access_token_secret = "dCZ80uNRbFDjxdU2EckmNiSckdoATach6Q8zb7YYYE5ER"
```


```python
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth)
```


```python
target_accounts = ("@BBCNews","@CBSNews","@CNN","@FoxNews","NYT")
sentiments_df = pd.DataFrame()
```


```python
for target in target_accounts:
    converted_timestamps = []
    tweet_text = []
    compound_list = []
    positive_list = []
    negative_list = []
    neutral_list = []

    for tweet in tweepy.Cursor(api.user_timeline, id=target).items(100):
        converted_time = datetime.strptime(str(tweet.created_at), "%Y-%m-%d %H:%M:%S")
        converted_timestamps.append(converted_time)

        tweet_text.append(tweet.text)

        compound = analyzer.polarity_scores(tweet.text)["compound"]
        pos = analyzer.polarity_scores(tweet.text)["pos"]
        neu = analyzer.polarity_scores(tweet.text)["neu"]
        neg = analyzer.polarity_scores(tweet.text)["neg"]
            
        compound_list.append(compound)
        positive_list.append(pos)
        negative_list.append(neg)
        neutral_list.append(neu)



    sentiments_df[target[1:] + 'Times Stamp'] = converted_timestamps
    sentiments_df[target[1:] + 'Tweet Text'] = tweet_text
    sentiments_df[target[1:] + 'Compound'] = compound_list
    sentiments_df[target[1:] + 'Positive'] = positive_list
    sentiments_df[target[1:] + 'Negative'] = negative_list
    sentiments_df[target[1:] + 'Neutral'] = neutral_list

sentiments_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BBCNewsTimes Stamp</th>
      <th>BBCNewsTweet Text</th>
      <th>BBCNewsCompound</th>
      <th>BBCNewsPositive</th>
      <th>BBCNewsNegative</th>
      <th>BBCNewsNeutral</th>
      <th>CBSNewsTimes Stamp</th>
      <th>CBSNewsTweet Text</th>
      <th>CBSNewsCompound</th>
      <th>CBSNewsPositive</th>
      <th>...</th>
      <th>FoxNewsCompound</th>
      <th>FoxNewsPositive</th>
      <th>FoxNewsNegative</th>
      <th>FoxNewsNeutral</th>
      <th>YTTimes Stamp</th>
      <th>YTTweet Text</th>
      <th>YTCompound</th>
      <th>YTPositive</th>
      <th>YTNegative</th>
      <th>YTNeutral</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2018-01-15 19:31:52</td>
      <td>RT @BBCBusiness: What does the #Carillion coll...</td>
      <td>-0.4939</td>
      <td>0.000</td>
      <td>0.198</td>
      <td>0.802</td>
      <td>2018-01-15 19:37:30</td>
      <td>U.S. moves ships, bombers toward Korea ahead o...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.3818</td>
      <td>0.133</td>
      <td>0.000</td>
      <td>0.867</td>
      <td>2018-01-15 19:33:47</td>
      <td>Critic’s Notebook: What a Hologram of Maria Ca...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2018-01-15 19:29:39</td>
      <td>RT @BBCNewsbeat: Cyrille Regis was an inspirat...</td>
      <td>0.5267</td>
      <td>0.175</td>
      <td>0.000</td>
      <td>0.825</td>
      <td>2018-01-15 18:55:38</td>
      <td>Hundreds of South Florida Haitians are protest...</td>
      <td>-0.4215</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.8173</td>
      <td>0.306</td>
      <td>0.000</td>
      <td>0.694</td>
      <td>2018-01-15 19:32:47</td>
      <td>‘The Greatest Showman’ Soundtrack Repeats at N...</td>
      <td>0.4588</td>
      <td>0.313</td>
      <td>0.164</td>
      <td>0.522</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2018-01-15 18:53:57</td>
      <td>Rape case collapses after 'cuddling' photos em...</td>
      <td>-0.7845</td>
      <td>0.000</td>
      <td>0.535</td>
      <td>0.465</td>
      <td>2018-01-15 18:40:33</td>
      <td>Dolores O'Riordan, singer of The Cranberries, ...</td>
      <td>-0.8020</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.1779</td>
      <td>0.190</td>
      <td>0.125</td>
      <td>0.684</td>
      <td>2018-01-15 19:16:47</td>
      <td>Asia and Australia Edition: Jakarta, North Kor...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2018-01-15 18:11:51</td>
      <td>Carillion collapse: Ministers hold emergency m...</td>
      <td>-0.7003</td>
      <td>0.000</td>
      <td>0.537</td>
      <td>0.463</td>
      <td>2018-01-15 18:25:45</td>
      <td>Take a road trip on this U.S. Civil Rights Tra...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.3810</td>
      <td>0.175</td>
      <td>0.000</td>
      <td>0.825</td>
      <td>2018-01-15 19:02:47</td>
      <td>Study Finds Increasing Diversity on Broadway. ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-01-15 18:11:51</td>
      <td>Westminster attacker 'took steroids' before te...</td>
      <td>-0.7964</td>
      <td>0.000</td>
      <td>0.542</td>
      <td>0.458</td>
      <td>2018-01-15 18:10:00</td>
      <td>What Facebook's news feed change means for use...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.8271</td>
      <td>0.000</td>
      <td>0.312</td>
      <td>0.688</td>
      <td>2018-01-15 18:47:47</td>
      <td>Challenging the Afghan State, a Cornered Stron...</td>
      <td>0.0516</td>
      <td>0.266</td>
      <td>0.169</td>
      <td>0.565</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2018-01-15 17:43:32</td>
      <td>Steven Seagal denies Bond girl assault https:/...</td>
      <td>-0.7650</td>
      <td>0.000</td>
      <td>0.569</td>
      <td>0.431</td>
      <td>2018-01-15 17:51:33</td>
      <td>Dolores O'Riordan, the lead singer of Irish ba...</td>
      <td>-0.5574</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.2263</td>
      <td>0.174</td>
      <td>0.216</td>
      <td>0.610</td>
      <td>2018-01-15 18:46:47</td>
      <td>On Baseball: What Is Baseball’s Equivalent to ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2018-01-15 17:22:12</td>
      <td>Barry Bennell: Ex-coach 'told me his cancer wa...</td>
      <td>-0.6597</td>
      <td>0.000</td>
      <td>0.328</td>
      <td>0.672</td>
      <td>2018-01-15 17:40:25</td>
      <td>"Hate cannot drive out hate, only love can do ...</td>
      <td>0.7429</td>
      <td>0.326</td>
      <td>...</td>
      <td>-0.5423</td>
      <td>0.000</td>
      <td>0.189</td>
      <td>0.811</td>
      <td>2018-01-15 18:33:47</td>
      <td>Dolores O’Riordan, Lead Singer of the Cranberr...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2018-01-15 17:20:09</td>
      <td>RT @BBCBreaking: Cranberries lead singer Dolor...</td>
      <td>-0.5574</td>
      <td>0.000</td>
      <td>0.159</td>
      <td>0.841</td>
      <td>2018-01-15 17:20:02</td>
      <td>Turkish plane's right engine experienced a sud...</td>
      <td>-0.4588</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.6124</td>
      <td>0.000</td>
      <td>0.286</td>
      <td>0.714</td>
      <td>2018-01-15 18:32:47</td>
      <td>Books of The Times: Catching Up With Denis Joh...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2018-01-15 17:14:29</td>
      <td>Cranberries singer Dolores O'Riordan dies http...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 17:01:01</td>
      <td>President Trummp has tweeted that he used "tou...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.5106</td>
      <td>0.268</td>
      <td>0.000</td>
      <td>0.732</td>
      <td>2018-01-15 18:31:47</td>
      <td>Ms. Fendi and Ms. Prada and All Their Baggage....</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2018-01-15 16:59:21</td>
      <td>RT @BBCNewsGraphics: Carillion buckled under t...</td>
      <td>-0.3612</td>
      <td>0.000</td>
      <td>0.106</td>
      <td>0.894</td>
      <td>2018-01-15 16:42:31</td>
      <td>These bearded "Merb'ys" (aka Mermaid boys) are...</td>
      <td>0.3182</td>
      <td>0.155</td>
      <td>...</td>
      <td>0.4995</td>
      <td>0.204</td>
      <td>0.083</td>
      <td>0.713</td>
      <td>2018-01-15 18:19:47</td>
      <td>Knife Attack at Russian School Leaves at Least...</td>
      <td>-0.4767</td>
      <td>0.000</td>
      <td>0.237</td>
      <td>0.763</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2018-01-15 16:47:24</td>
      <td>RT @bbcweather: Here's the set up for rush hou...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 16:40:01</td>
      <td>Iranian tanker goes down in flames, leaving a ...</td>
      <td>-0.5423</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0243</td>
      <td>0.132</td>
      <td>0.127</td>
      <td>0.741</td>
      <td>2018-01-15 18:18:47</td>
      <td>A Blot on Ireland’s Past, Facing Demolition. h...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2018-01-15 16:43:00</td>
      <td>"Our top priority is to safeguard the continui...</td>
      <td>0.0516</td>
      <td>0.186</td>
      <td>0.136</td>
      <td>0.678</td>
      <td>2018-01-15 16:22:25</td>
      <td>U.K. politician says he has broken up with his...</td>
      <td>-0.7964</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 18:16:47</td>
      <td>Dolores O’Riordan, Lead Singer of The Cranberr...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2018-01-15 16:30:54</td>
      <td>RT @BBCNewsround: What's life like for Rohingy...</td>
      <td>0.3612</td>
      <td>0.128</td>
      <td>0.000</td>
      <td>0.872</td>
      <td>2018-01-15 16:02:01</td>
      <td>Trust of media takes a hit during first year o...</td>
      <td>0.2500</td>
      <td>0.146</td>
      <td>...</td>
      <td>0.3400</td>
      <td>0.179</td>
      <td>0.000</td>
      <td>0.821</td>
      <td>2018-01-15 17:58:49</td>
      <td>Front Burner: Canada’s Oysters: Treasures Read...</td>
      <td>0.6486</td>
      <td>0.398</td>
      <td>0.000</td>
      <td>0.602</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2018-01-15 16:15:12</td>
      <td>Follow the latest #Carillion developments \n\n...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 15:42:01</td>
      <td>Female passenger dies after casino boat fire f...</td>
      <td>-0.3400</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.6486</td>
      <td>0.000</td>
      <td>0.264</td>
      <td>0.736</td>
      <td>2018-01-15 17:57:47</td>
      <td>Front Burner: A Buttery Spanish Cheese for Mel...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2018-01-15 16:03:07</td>
      <td>House of Commons committee to hold inquiry int...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 15:22:01</td>
      <td>President Trump remarks polarize conservative ...</td>
      <td>-0.6124</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.7196</td>
      <td>0.273</td>
      <td>0.000</td>
      <td>0.727</td>
      <td>2018-01-15 17:55:47</td>
      <td>Front Burner: Dinners Showcase the Flavors of ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2018-01-15 15:28:26</td>
      <td>RT @BBCSport: The ECB says it will decide with...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 15:10:58</td>
      <td>A moment of silence followed by a prayer is he...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 17:52:47</td>
      <td>Graydon Carter, Ex-Editor of Vanity Fair, Inve...</td>
      <td>0.1027</td>
      <td>0.174</td>
      <td>0.144</td>
      <td>0.682</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2018-01-15 15:26:41</td>
      <td>RT @BBC_HaveYourSay: Carillion - "Everyone on ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 15:07:45</td>
      <td>“We are going to be a great generation,” Marti...</td>
      <td>0.6249</td>
      <td>0.170</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 17:50:47</td>
      <td>Unbuttoned: Airbrushing Meets the #MeToo Movem...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2018-01-15 15:17:14</td>
      <td>RT @bbcthree: London is experiencing a huge sp...</td>
      <td>-0.1531</td>
      <td>0.104</td>
      <td>0.131</td>
      <td>0.766</td>
      <td>2018-01-15 15:06:31</td>
      <td>“What I am here to do is to look to the future...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.1779</td>
      <td>0.145</td>
      <td>0.000</td>
      <td>0.855</td>
      <td>2018-01-15 17:49:48</td>
      <td>Philippines Shuts Down News Site Critical of R...</td>
      <td>-0.3182</td>
      <td>0.000</td>
      <td>0.204</td>
      <td>0.796</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2018-01-15 14:35:22</td>
      <td>RT @richard_conway: ECB on Stokes: "aware.. ch...</td>
      <td>-0.2023</td>
      <td>0.000</td>
      <td>0.076</td>
      <td>0.924</td>
      <td>2018-01-15 15:02:47</td>
      <td>WATCH: Wreath-laying ceremony at Martin Luther...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.2960</td>
      <td>0.171</td>
      <td>0.300</td>
      <td>0.529</td>
      <td>2018-01-15 17:48:47</td>
      <td>Balcony at Indonesian Stock Exchange in Jakart...</td>
      <td>-0.2960</td>
      <td>0.000</td>
      <td>0.216</td>
      <td>0.784</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2018-01-15 14:21:42</td>
      <td>RT @BBCWorld: Extraordinary footage shows the ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 15:02:31</td>
      <td>WATCH: A second-story floor collapses undernea...</td>
      <td>-0.2960</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.6369</td>
      <td>0.000</td>
      <td>0.286</td>
      <td>0.714</td>
      <td>2018-01-15 17:47:47</td>
      <td>The Week Ahead: More Bank Earnings, China’s Gr...</td>
      <td>0.3818</td>
      <td>0.178</td>
      <td>0.000</td>
      <td>0.822</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2018-01-15 14:07:37</td>
      <td>RT @DannyShawBBC: FROM CPS: “Ben Stokes, 26, R...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 14:42:01</td>
      <td>Russia, Turkey and the Syrian government denou...</td>
      <td>-0.6908</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.5719</td>
      <td>0.291</td>
      <td>0.000</td>
      <td>0.709</td>
      <td>2018-01-15 17:46:47</td>
      <td>Front Burner: Cooking, the Benjamin Franklin W...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2018-01-15 14:06:02</td>
      <td>Ben Stokes: England cricketer charged with aff...</td>
      <td>-0.2023</td>
      <td>0.000</td>
      <td>0.205</td>
      <td>0.795</td>
      <td>2018-01-15 14:22:38</td>
      <td>WATCH: A firefighter makes a life-saving catch...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.3612</td>
      <td>0.000</td>
      <td>0.172</td>
      <td>0.828</td>
      <td>2018-01-15 17:45:47</td>
      <td>Front Burner: An Old New York Taste by Way of ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2018-01-15 13:14:22</td>
      <td>Ryan Giggs: Manchester United legend named Wal...</td>
      <td>0.4215</td>
      <td>0.259</td>
      <td>0.000</td>
      <td>0.741</td>
      <td>2018-01-15 14:02:01</td>
      <td>"Several victims" reported in shooting on San ...</td>
      <td>-0.3182</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 17:15:47</td>
      <td>The Healing Edge: After Surgery in the Womb, a...</td>
      <td>0.4404</td>
      <td>0.195</td>
      <td>0.000</td>
      <td>0.805</td>
    </tr>
    <tr>
      <th>23</th>
      <td>2018-01-15 13:11:59</td>
      <td>RT @BBCSportWales: Manchester United legend Ry...</td>
      <td>0.7184</td>
      <td>0.240</td>
      <td>0.000</td>
      <td>0.760</td>
      <td>2018-01-15 13:47:39</td>
      <td>Navigation apps like Waze and Google Maps can ...</td>
      <td>0.6369</td>
      <td>0.206</td>
      <td>...</td>
      <td>-0.5267</td>
      <td>0.000</td>
      <td>0.306</td>
      <td>0.694</td>
      <td>2018-01-15 17:02:47</td>
      <td>Victoria Beckham Draws Uproar Over Superthin M...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2018-01-15 12:44:41</td>
      <td>Momentum founder Jon Lansman voted on to Labou...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 13:44:39</td>
      <td>The tourism industry is worried about a new go...</td>
      <td>-0.3818</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 17:00:47</td>
      <td>Front Burner: A Brunch Where the Mole Flows. h...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2018-01-15 12:44:41</td>
      <td>RAF intercepts Russian bombers near UK 'area o...</td>
      <td>0.4588</td>
      <td>0.250</td>
      <td>0.000</td>
      <td>0.750</td>
      <td>2018-01-15 13:42:01</td>
      <td>From taxes to immigration, Pres. Trump has fol...</td>
      <td>0.3818</td>
      <td>0.120</td>
      <td>...</td>
      <td>-0.7148</td>
      <td>0.000</td>
      <td>0.317</td>
      <td>0.683</td>
      <td>2018-01-15 16:17:47</td>
      <td>‘I’m Not a Racist’. https://t.co/VCNn7qwDx8</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2018-01-15 12:02:12</td>
      <td>Man denies Buckingham Palace sword terror char...</td>
      <td>-0.7351</td>
      <td>0.000</td>
      <td>0.508</td>
      <td>0.492</td>
      <td>2018-01-15 13:41:01</td>
      <td>Commentary: "It's the economy, stupid" could b...</td>
      <td>-0.5267</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.1027</td>
      <td>0.115</td>
      <td>0.138</td>
      <td>0.747</td>
      <td>2018-01-15 15:59:47</td>
      <td>Lesotho Diamond Weighs More Than a Baseball. h...</td>
      <td>0.3400</td>
      <td>0.286</td>
      <td>0.000</td>
      <td>0.714</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2018-01-15 11:41:48</td>
      <td>RT @bbcweather: Blustery showers for many of u...</td>
      <td>0.3612</td>
      <td>0.135</td>
      <td>0.000</td>
      <td>0.865</td>
      <td>2018-01-15 13:39:39</td>
      <td>In the @CBSThisMorning series #AmericanVoices,...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.3400</td>
      <td>0.000</td>
      <td>0.405</td>
      <td>0.595</td>
      <td>2018-01-15 15:58:47</td>
      <td>What to Cook: A Cake for Dr. King. https://t.c...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2018-01-15 11:39:43</td>
      <td>Paul Lambert appointed Stoke manager https://t...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 13:34:19</td>
      <td>Here’s a look at some of this morning’s other ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.4588</td>
      <td>0.143</td>
      <td>0.000</td>
      <td>0.857</td>
      <td>2018-01-15 15:45:46</td>
      <td>Music Therapy Offers an End-of-Life Grace Note...</td>
      <td>0.4215</td>
      <td>0.286</td>
      <td>0.000</td>
      <td>0.714</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2018-01-15 11:32:06</td>
      <td>RT @BBCSport: Paul Lambert has been named the ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 13:29:03</td>
      <td>In honor of the Martin Luther King Jr. holiday...</td>
      <td>0.7096</td>
      <td>0.258</td>
      <td>...</td>
      <td>-0.3182</td>
      <td>0.000</td>
      <td>0.161</td>
      <td>0.839</td>
      <td>2018-01-15 15:44:47</td>
      <td>Review: ‘Black Lightning’ Is Pulp With a Purpo...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>70</th>
      <td>2018-01-14 16:25:17</td>
      <td>Redditch parents mourn third child's heart dea...</td>
      <td>-0.7717</td>
      <td>0.000</td>
      <td>0.528</td>
      <td>0.472</td>
      <td>2018-01-15 08:18:07</td>
      <td>Oprah Winfrey on "Time's Up": Where do we go f...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.4939</td>
      <td>0.198</td>
      <td>0.000</td>
      <td>0.802</td>
      <td>2018-01-15 00:11:43</td>
      <td>Graydon Carter, Ex-Editor of Vanity Fair, Inve...</td>
      <td>0.1027</td>
      <td>0.174</td>
      <td>0.144</td>
      <td>0.682</td>
    </tr>
    <tr>
      <th>71</th>
      <td>2018-01-14 16:25:17</td>
      <td>Falklands War 'true hero' Captain Rick Jolly d...</td>
      <td>0.7003</td>
      <td>0.522</td>
      <td>0.210</td>
      <td>0.269</td>
      <td>2018-01-15 08:03:05</td>
      <td>The show-off state https://t.co/mF4JYFdPsY htt...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-14 23:53:44</td>
      <td>How a Team of New York City Inspectors Helped ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>72</th>
      <td>2018-01-14 16:25:17</td>
      <td>Londoners hit out at 'mistimed' bus safety ale...</td>
      <td>0.4215</td>
      <td>0.259</td>
      <td>0.000</td>
      <td>0.741</td>
      <td>2018-01-15 07:48:05</td>
      <td>Sen. Joe Manchin: No senator would make up "at...</td>
      <td>-0.2960</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.7269</td>
      <td>0.108</td>
      <td>0.329</td>
      <td>0.563</td>
      <td>2018-01-14 23:52:43</td>
      <td>Metropolitan Diary: Ciao Trader Vic’s, Hello T...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>73</th>
      <td>2018-01-14 15:17:43</td>
      <td>Puppy in Simon Cowell reward offer turns up sa...</td>
      <td>0.7650</td>
      <td>0.398</td>
      <td>0.000</td>
      <td>0.602</td>
      <td>2018-01-15 07:42:12</td>
      <td>New details in police chase of Greyhound bus w...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.3400</td>
      <td>0.097</td>
      <td>0.188</td>
      <td>0.714</td>
      <td>2018-01-14 23:51:43</td>
      <td>Liberia President’s Ouster by Party May Raise ...</td>
      <td>0.4019</td>
      <td>0.197</td>
      <td>0.000</td>
      <td>0.803</td>
    </tr>
    <tr>
      <th>74</th>
      <td>2018-01-14 14:36:45</td>
      <td>RT @BBCBreaking: A 7.1 magnitude quake off the...</td>
      <td>-0.5423</td>
      <td>0.000</td>
      <td>0.200</td>
      <td>0.800</td>
      <td>2018-01-15 07:32:05</td>
      <td>Jakarta Stock Exchange mezzanine collapse caus...</td>
      <td>-0.7845</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.4019</td>
      <td>0.144</td>
      <td>0.000</td>
      <td>0.856</td>
      <td>2018-01-14 23:22:43</td>
      <td>Tunisia’s Government Pledges Improvements Afte...</td>
      <td>0.1027</td>
      <td>0.250</td>
      <td>0.207</td>
      <td>0.543</td>
    </tr>
    <tr>
      <th>75</th>
      <td>2018-01-14 13:00:52</td>
      <td>Cassie Hayes death: Tribute to Tui Southport t...</td>
      <td>-0.5994</td>
      <td>0.000</td>
      <td>0.302</td>
      <td>0.698</td>
      <td>2018-01-15 07:18:09</td>
      <td>Sen. Tom Cotton says Trump's words, sentiments...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-14 23:09:42</td>
      <td>In Iran, Protester ‘Suicides’ Stir Anger and C...</td>
      <td>-0.5719</td>
      <td>0.000</td>
      <td>0.270</td>
      <td>0.730</td>
    </tr>
    <tr>
      <th>76</th>
      <td>2018-01-14 12:20:00</td>
      <td>Australia v England: Jason Roy hits record 180...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 07:03:08</td>
      <td>2 children among the dead in triple homicide i...</td>
      <td>-0.6486</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-14 23:08:43</td>
      <td>Jaguars 45, Steelers 42 | Divisional round: Ja...</td>
      <td>0.4588</td>
      <td>0.235</td>
      <td>0.000</td>
      <td>0.765</td>
    </tr>
    <tr>
      <th>77</th>
      <td>2018-01-14 12:20:00</td>
      <td>Anthony Joshua v Joseph Parker fight announced...</td>
      <td>-0.3818</td>
      <td>0.000</td>
      <td>0.206</td>
      <td>0.794</td>
      <td>2018-01-15 06:54:05</td>
      <td>Double suicide bombing in Baghdad takes deadly...</td>
      <td>-0.6705</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.3612</td>
      <td>0.116</td>
      <td>0.000</td>
      <td>0.884</td>
      <td>2018-01-14 23:07:43</td>
      <td>Ciao Trader Vic’s, Hello Trader Joe’s. https:/...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>78</th>
      <td>2018-01-14 11:58:36</td>
      <td>RT @BBCSport: Jason Roy hit a record-breaking ...</td>
      <td>0.4019</td>
      <td>0.114</td>
      <td>0.000</td>
      <td>0.886</td>
      <td>2018-01-15 06:48:05</td>
      <td>Speeding car goes airborne, crashes into secon...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.3612</td>
      <td>0.116</td>
      <td>0.000</td>
      <td>0.884</td>
      <td>2018-01-14 22:52:42</td>
      <td>It’s Elementary: Sherlockians Take Manhattan. ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>79</th>
      <td>2018-01-14 11:48:36</td>
      <td>One in 10 renters have fire risk concerns, pol...</td>
      <td>-0.5423</td>
      <td>0.000</td>
      <td>0.333</td>
      <td>0.667</td>
      <td>2018-01-15 06:33:07</td>
      <td>Faith Salie on when POTUS uses "$#!?hole" lang...</td>
      <td>0.4753</td>
      <td>0.256</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-14 22:22:42</td>
      <td>Response to French Letter Denouncing #MeToo Sh...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>80</th>
      <td>2018-01-14 11:10:25</td>
      <td>Mario Testino and Bruce Weber accused of sexua...</td>
      <td>-0.2960</td>
      <td>0.000</td>
      <td>0.196</td>
      <td>0.804</td>
      <td>2018-01-15 06:18:06</td>
      <td>Nation Tracker: Americans weigh in on Trump im...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.1779</td>
      <td>0.000</td>
      <td>0.134</td>
      <td>0.866</td>
      <td>2018-01-14 22:21:42</td>
      <td>In Czech Election, a Choice Between Leaning Ea...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>81</th>
      <td>2018-01-14 11:10:25</td>
      <td>Teenager stabbed to death at Walsall house par...</td>
      <td>-0.6249</td>
      <td>0.174</td>
      <td>0.439</td>
      <td>0.387</td>
      <td>2018-01-15 06:03:05</td>
      <td>For chef Fabio Trabocchi, cooking is a family ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.0900</td>
      <td>0.175</td>
      <td>0.241</td>
      <td>0.584</td>
      <td>2018-01-14 20:40:42</td>
      <td>He Studied Accounting. Now He Hunts the Taliba...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>82</th>
      <td>2018-01-14 11:10:25</td>
      <td>Glasgow bin lorry family's £800k compensation ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 05:48:05</td>
      <td>Married couple killed as Amtrak train hits SUV...</td>
      <td>-0.6705</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.125</td>
      <td>0.125</td>
      <td>0.750</td>
      <td>2018-01-14 20:04:42</td>
      <td>‘Jumanji’ Tops Box Office for a Second Week. h...</td>
      <td>0.5106</td>
      <td>0.320</td>
      <td>0.000</td>
      <td>0.680</td>
    </tr>
    <tr>
      <th>83</th>
      <td>2018-01-14 10:46:17</td>
      <td>Madeleine McCann private detective Kevin Halli...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 05:36:38</td>
      <td>Motorist strikes officer before speeding off i...</td>
      <td>-0.3612</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.6124</td>
      <td>0.000</td>
      <td>0.267</td>
      <td>0.733</td>
      <td>2018-01-14 19:35:42</td>
      <td>Asia and Australia Edition: Hawaii, North Kore...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>84</th>
      <td>2018-01-14 10:35:25</td>
      <td>RT @bbcnickrobinson: Labour leaves door open t...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 05:28:06</td>
      <td>Woman in Michigan faces charges in death of 2n...</td>
      <td>-0.7184</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.4939</td>
      <td>0.198</td>
      <td>0.000</td>
      <td>0.802</td>
      <td>2018-01-14 18:55:41</td>
      <td>Australia Diary: There Are Poets Among Us. htt...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>85</th>
      <td>2018-01-14 10:07:55</td>
      <td>Independence for Scotland is not a "constituti...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 05:18:08</td>
      <td>Transcript: Sen. Tom Cotton on "Face the Natio...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.4404</td>
      <td>0.000</td>
      <td>0.172</td>
      <td>0.828</td>
      <td>2018-01-14 18:51:41</td>
      <td>On Soccer: Liverpool Beats Manchester City, De...</td>
      <td>0.4939</td>
      <td>0.211</td>
      <td>0.000</td>
      <td>0.789</td>
    </tr>
    <tr>
      <th>86</th>
      <td>2018-01-14 09:59:20</td>
      <td>RT @MarrShow: "Do you accept Scotland is going...</td>
      <td>0.6486</td>
      <td>0.206</td>
      <td>0.045</td>
      <td>0.749</td>
      <td>2018-01-15 05:03:07</td>
      <td>Vikings stun Saints, 29-24, with 61-yard touch...</td>
      <td>0.3400</td>
      <td>0.179</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-14 18:37:41</td>
      <td>Detroit Auto Show May Be Celebrating an Era Ab...</td>
      <td>0.5719</td>
      <td>0.252</td>
      <td>0.000</td>
      <td>0.748</td>
    </tr>
    <tr>
      <th>87</th>
      <td>2018-01-14 09:53:08</td>
      <td>Staying in the single market &amp;amp; the customs...</td>
      <td>0.4023</td>
      <td>0.131</td>
      <td>0.000</td>
      <td>0.869</td>
      <td>2018-01-15 04:48:04</td>
      <td>The American scientist who's seen North Korea'...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.5994</td>
      <td>0.000</td>
      <td>0.290</td>
      <td>0.710</td>
      <td>2018-01-14 18:20:41</td>
      <td>Review: Risk-Taking New Opera Tells a Tragic 1...</td>
      <td>0.2960</td>
      <td>0.259</td>
      <td>0.185</td>
      <td>0.556</td>
    </tr>
    <tr>
      <th>88</th>
      <td>2018-01-14 09:47:41</td>
      <td>Conservative Party chairman Brandon Lewis says...</td>
      <td>-0.2732</td>
      <td>0.208</td>
      <td>0.251</td>
      <td>0.541</td>
      <td>2018-01-15 04:33:07</td>
      <td>Media outlets around the world struggle to tra...</td>
      <td>-0.3182</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.5267</td>
      <td>0.000</td>
      <td>0.298</td>
      <td>0.702</td>
      <td>2018-01-14 18:05:41</td>
      <td>1 Dead as Earthquake Strikes Peru. https://t.c...</td>
      <td>-0.7783</td>
      <td>0.000</td>
      <td>0.630</td>
      <td>0.370</td>
    </tr>
    <tr>
      <th>89</th>
      <td>2018-01-14 09:44:00</td>
      <td>RT @MarrShow: "The Secretary of State for Just...</td>
      <td>0.6908</td>
      <td>0.206</td>
      <td>0.000</td>
      <td>0.794</td>
      <td>2018-01-15 04:18:12</td>
      <td>Plane skids off runway in Turkey, comes to a r...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.7351</td>
      <td>0.000</td>
      <td>0.437</td>
      <td>0.563</td>
      <td>2018-01-14 17:50:41</td>
      <td>Review: Without Singing, the Moth Hits the Hig...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>90</th>
      <td>2018-01-14 09:34:47</td>
      <td>Carillion crunch rescue talks to resume https:...</td>
      <td>0.5106</td>
      <td>0.355</td>
      <td>0.000</td>
      <td>0.645</td>
      <td>2018-01-15 04:03:04</td>
      <td>Reflections from Afghanistan https://t.co/awlH...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.5574</td>
      <td>0.205</td>
      <td>0.000</td>
      <td>0.795</td>
      <td>2018-01-14 17:40:41</td>
      <td>Big Bets on A.I. Open a New Frontier for Chip ...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>91</th>
      <td>2018-01-14 09:33:28</td>
      <td>RT @BBCBreaking: Burning oil tanker sinks more...</td>
      <td>-0.5859</td>
      <td>0.000</td>
      <td>0.186</td>
      <td>0.814</td>
      <td>2018-01-15 03:48:06</td>
      <td>Drawing the lines on gerrymandering https://t....</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-14 17:37:41</td>
      <td>Plane Skids Off Runway in Turkey, Teetering on...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>92</th>
      <td>2018-01-14 09:29:15</td>
      <td>If we "hack off" Donald Trump won't it be hard...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 03:46:04</td>
      <td>Aziz Ansari responds to woman's claim of sexua...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-14 17:36:41</td>
      <td>Hope Fades for Missing Crew Members as Iranian...</td>
      <td>0.1779</td>
      <td>0.192</td>
      <td>0.146</td>
      <td>0.662</td>
    </tr>
    <tr>
      <th>93</th>
      <td>2018-01-14 09:28:04</td>
      <td>RT @MarrShow: Worboys release is a "threat to ...</td>
      <td>-0.7855</td>
      <td>0.080</td>
      <td>0.297</td>
      <td>0.623</td>
      <td>2018-01-15 03:33:05</td>
      <td>Oprah Winfrey on "Time's Up": Where do we go f...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.3400</td>
      <td>0.112</td>
      <td>0.000</td>
      <td>0.888</td>
      <td>2018-01-14 17:35:41</td>
      <td>In Some Countries, Facebook’s Fiddling Has Mag...</td>
      <td>-0.4767</td>
      <td>0.000</td>
      <td>0.256</td>
      <td>0.744</td>
    </tr>
    <tr>
      <th>94</th>
      <td>2018-01-14 09:20:17</td>
      <td>Fight Labour online, Tory chairman Brandon Lew...</td>
      <td>-0.3818</td>
      <td>0.000</td>
      <td>0.245</td>
      <td>0.755</td>
      <td>2018-01-15 03:18:10</td>
      <td>Sen. Joe Manchin: No senator would make up "at...</td>
      <td>-0.2960</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.3818</td>
      <td>0.000</td>
      <td>0.178</td>
      <td>0.822</td>
      <td>2018-01-14 17:20:41</td>
      <td>A Heart-Stopping Skid in Turkey. https://t.co/...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>95</th>
      <td>2018-01-14 03:00:30</td>
      <td>Sturgeon: UK Brexit plan 'beggars belief' http...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 03:14:05</td>
      <td>U.K. party suspends leader's girlfriend over r...</td>
      <td>-0.3182</td>
      <td>0.162</td>
      <td>...</td>
      <td>-0.5574</td>
      <td>0.000</td>
      <td>0.194</td>
      <td>0.806</td>
      <td>2018-01-14 17:10:41</td>
      <td>G.O.P. Senator Says Trump Didn’t Use Vulgarity...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>96</th>
      <td>2018-01-14 02:53:28</td>
      <td>UKIP suspends leader's girlfriend after Meghan...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 03:03:05</td>
      <td>Jaguars edge out Steelers 45-42, will meet Pat...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.4588</td>
      <td>0.158</td>
      <td>0.000</td>
      <td>0.842</td>
      <td>2018-01-14 17:05:41</td>
      <td>In Milan, Military Rigor Meets Instagram Exces...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>97</th>
      <td>2018-01-14 01:33:31</td>
      <td>Queen's coronation details revealed in documen...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 02:57:26</td>
      <td>"I am not a racist. I am the least racist pers...</td>
      <td>0.6298</td>
      <td>0.250</td>
      <td>...</td>
      <td>0.4973</td>
      <td>0.152</td>
      <td>0.000</td>
      <td>0.848</td>
      <td>2018-01-14 16:54:41</td>
      <td>Advertising: Don’t Listen to Washington, Touri...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
    <tr>
      <th>98</th>
      <td>2018-01-14 00:05:13</td>
      <td>Chef Angela Hartnett says Britain is not a 'fo...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 02:40:01</td>
      <td>Detectives probe claim that disgraced former p...</td>
      <td>-0.8934</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0000</td>
      <td>0.125</td>
      <td>0.125</td>
      <td>0.750</td>
      <td>2018-01-14 16:53:41</td>
      <td>Rishikesh Journal: Rebuilding on the Beatles, ...</td>
      <td>0.4215</td>
      <td>0.177</td>
      <td>0.000</td>
      <td>0.823</td>
    </tr>
    <tr>
      <th>99</th>
      <td>2018-01-13 22:11:23</td>
      <td>John Worboys: Minister considering judicial re...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>2018-01-15 02:21:32</td>
      <td>President Trump says he's not a racist days af...</td>
      <td>0.4973</td>
      <td>0.145</td>
      <td>...</td>
      <td>-0.2924</td>
      <td>0.000</td>
      <td>0.166</td>
      <td>0.834</td>
      <td>2018-01-14 16:52:41</td>
      <td>At Milan Men’s Wear, Military Rigor Meets Inst...</td>
      <td>0.0000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>1.000</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 30 columns</p>
</div>




```python
sentiments_df.to_csv("News Mood.csv")
```


```python
plot_df=sentiments_df[[sentiments_df.columns[2], sentiments_df.columns[8], sentiments_df.columns[14], sentiments_df.columns[20], sentiments_df.columns[26]]]
plot_df=plot_df.rename(index=str,columns={'BBCNewsCompound':'BBC',
                                         'CBSNewsCompound':'CBS',
                                         'CNNCompound':'CNN',
                                          'FoxNewsCompound':'Fox',
                                         'YTCompound':'New York Times'})
plot_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>BBC</th>
      <th>CBS</th>
      <th>CNN</th>
      <th>Fox</th>
      <th>New York Times</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.4939</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>0.3818</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.5267</td>
      <td>-0.4215</td>
      <td>0.0000</td>
      <td>0.8173</td>
      <td>0.4588</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.7845</td>
      <td>-0.8020</td>
      <td>-0.6808</td>
      <td>0.1779</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.7003</td>
      <td>0.0000</td>
      <td>0.4310</td>
      <td>0.3810</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.7964</td>
      <td>0.0000</td>
      <td>0.0000</td>
      <td>-0.8271</td>
      <td>0.0516</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, ax = plt.subplots(figsize=(8,6))
for column in plot_df.columns:
    ax.scatter(x=plot_df.index, y=plot_df[column], zorder=5)
    
ax.set_title('Sentiment Analysis of Media Tweets (01/15/18)')
ax.set_xlabel('Tweets Ago')
ax.set_ylabel('Tweet Polarity')
ax.set_xlim(0,100)
ax.set_ylim(-1,1)

tick_locations = [0,20,40,60,80,100]
plt.xticks(tick_locations,['0','20','40','60','80','100'])

box=ax.get_position()
ax.set_position([box.x0,box.y0,box.width,box.height])
ax.legend(bbox_to_anchor=(1.05,1),loc=2,title='Media Sources',fontsize=10)
plt.grid(True)
sns.set()

plt.show()
```


![png](output_7_0.png)



```python
fig, ax = plt.subplots(figsize=(10,6)) 

ax.bar(x=plot_df.columns, height=plot_df.mean(), width=0.8,color=plt.cm.Paired(np.arange(len(plot_df))))
ax.set_title('Overall Media Sentiment based on Twitter (01/15/2018)')
ax.set_ylabel('Tweet Polarity')
ax.set_ylim(-0.15, 0.1)

for bar in ax.patches:
    height=bar.get_height()
    if bar.get_height() < 0:
        label=height-0.01
    else:
        label=height+0.01
        
    ax.text(bar.get_x()+0.1, label,'{:.2f}'.format(height),ha='center',va='bottom')
    
    
plt.show()
```


![png](output_8_0.png)

