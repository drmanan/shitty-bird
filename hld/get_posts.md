# Design the ShittyBird feed (timeline)

This idea is similar to any social media feed or timeline.

## End point: **GET** ***/posts***

## Use case

> Gathering requirements and scope the problem.

### **SCOPE**

> Lets limit our thought process to just serving user a list of tweets here

- User can view the list of his personal tweets.
- User can view the list of tweets from another specific user.
  - Subject user identifier to be given.
- User can view the list home/timeline.
  - Unique to the user.
  - A list of the most recent tweets from the people they follow.
- High Availability of service: ***CAP***
- Analytics
  - Reply count
  - Shares (retweets)
  - Quotes
  - Likes
  - Viwes

#### Future scope

> *Can also be refered to as out of scope*

- Midea support
  - Images, 4 at most
  - Video+
  - URLs
- For user's home/timeline,
  - Tweets from influencers to be added to home feed, inspite of following or not following.
  - Add tweets from other users with
    - similar interests
    - user subscribed topics
- Adverts to be added to all feeds/timelines.
- Promoted tweets to be added to all feeds/timelines.
- Remove or hide tweets based on
  - Other users visibility
    - If user is **blocked**
    - If tweets have a visibility attribute
- More analytics
  - Possible audiences estimates

## Assumptions and calculations

- Twitter has 365 million monthly active users.
- These generate around 500 billion read requests per month.
- Viewing timeliness/feeds should be fast.
- Twitter is a **read-heavy** service.
  - Should be optimised for fast reading of tweets.

### Calculations

#### Size of a tweet

- tweet_id: Long Integer: 8 Bytes.
  - Using Long as Int has a limit of 2 billion digits.
  - Long has 9 quintillions: 9 x 10^18 possible values.
- text: String, max. 140 characters: 140 Bytes.
- user_id: The person who has tweeted this: Integer 4 Bytes.
  - User Id is in Integer, reasons are given [here](user.md).
- like_count: Integer: 4 Bytes.
- reply_count: Integer: 4 Bytes.
- viiw_count: Integer: 4 Bytes.
- shares_count: Integer: 4 Bytes.
- in_reply_to: Long Integer: 8 Bytes
  - tweet_id of a tweet that the user has replie to.
- timestamp: Long Integer: 8 Bytes.
  - in Epoch Time Seconds.
  - I am an advocate of epoch time as ISO time takes a lot more space
    - Consider `2000-10-31T01:30:00.000Z+05:30` is a 30-character string, hence takes atleast 30 Bytes if you caount the characters, This is a 32 Byte data structure in reality.
    - Epoch time Seconds take 8 Bytes.
    - Epoch and ISO have similar coverage
- **Total Size** : 184 Bytes, Considering other properties, lets round this off at ***200 Bytes***.
- Each request (page) will have around ten such tweets. hence the total size of each request will be 10 * 200 bytes = 2000 bytes and some matadata or about **2.5 Kb**.

--- 

#### Bandwidth - (unnessesary)

184 bytes per tweet, 500 Billion read requests per month.
500000000000 *10 /30 /24 /60 /60 = 1929000 ~ 2000000 or 2 million requests per second.

Bandwidth needs to be at least 36000000 Bytes per second. Or 340 MBpS.
