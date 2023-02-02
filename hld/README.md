# HIgh level design of ShittyBird (twitter clone)

## Assumptions and calculations

- Every second, on average, around ***6,000*** tweets.
  - or, 350,000 tweets sent per minute
  - or, 500 million tweets sent each day
  - or, **200 billion** tweets per year. 
[(source)](https://www.dsayce.com/social-media/tweets-day/)

- Twitter had 368 million monthly active users in 2022. [(source)](https://www.businessofapps.com/data/twitter-statistics/)

- Traffic is not evenly distributed.
- Posting a tweet should be fast enough.
  - Delivering a tweet to your followers should be fast enough unless you have followers in hunderd thousands or millions.
