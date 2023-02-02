# Design the ShittyBird feed (timeline)

This idea is similar to any social media feed or timeline.

## End point: **GET /posts**

## Use case

> Gathering requirements and scope the problem.

### **SCOPE**: Lets limit our thought process to just serving user a list of tweets here

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
