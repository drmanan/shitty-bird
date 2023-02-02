# ShittyBird -A Twitter clone for dissing posts

## Summary

- A twitter-like social media site where anyone can post 240-character texts (future: images), and people can reply, like or reshare the posts.
- Users can follow each other. The home feed shows posts from users that they follow.
- Users will be notified when someone likes their posts, replies or reshares them and when someone follows them.
- Users and posts can be searched.

### I will be disscussing both high-level (system design) and low-level design (software design) of a Twitter like platform.

#### // TODO: compose specific pages for the following heads

- Introduction
    1. Definition of High-level system design
    1. Overview of Twitter
- Overview of High-level System Design for Twitter
    1. Infrastructure
    1. Storage
    1. Data Processing
    1. Security
- System Architecture
    1. Client-Server Model
    1. Database Architecture
    1. Application Architecture
- Components of the System
    1. Frontend
    1. Backend
    1. APIs
    1. Databases

---

### Inspiration source

> [A discussion on Scaler Academy forum](https://github.com/scaleracademy/open-source-projects/discussions/81)

--

### Progress markers

⬜ TODO: Implement\
✅ DONE: progress

---

## Future Scope

- Report posts.
- User's can send personal messages to each other. ❓
  - Will use a seperate backend service (Chat feat.)
- Allow accounts to be private (only visible to followers). ❓
- Turn on notifications for a particular user. ❓
- Bookmarking posts. ❓
- Topics. ❓

---

## Entities (in product terms)

1. Users
     - username
     - avatar (display pic)
       - initially pick this from gravatar
     - name
     - email id
     - contact no
       - authenticating user
       - authentication of ***verified*** account
     - bio
2. Posts
     - author
     - text
     - image
       - upto 4, just as twitter
     - likes
     - reposts
       - views, retweets, quotes and likes
       - lets combine stats of retweets and quotes
     - comments
     - inReplyTo -> another post (or null)
     - timestamp
     - mentions (via @user)
     - hashtags (via #tags)
     - threads
3. Notification
     - to
     - post (if it is a like/reply/share)
     - message
4. Hashtags
    - title (lowercase strings)
    - Indipendently searchable and listable

---

## Database Design

## Tables

### **Users** ⬜

**column**|*definition*
-----|-----
id|uuid
username|string(30) - only alphanumeric
name|string (50)
avatar|string (url)
bio|text (240)
follower\_count|bigint
following\_count|bigint
verified|boolean

### **Posts** ⬜

**column**|*definition*
-----|-----
id|uuid
text|string (240) chars
author\_id|uuid - foreignkey(user.id)
images|array<string(url)> - max 4
like\_count|bigint
repost\_count|bigint
orig\_post\_id|uuid (self reference, can be null)
reply\_to\_id|uuid (self reference, can be null)
hashtags|array
mentions|array<{name: string, id: uuid}>

### **Hashtags** ⬜

| **column** |*definition* |
|---|---|
| id | uuid |
| tag | text (only a-z,0-9,_ chars) |
| recent_post_count | bigint |

### **Likes** ⬜

|**column**|*definition*               |
|--------|-----------------------------|
| id     | uuid                        |
| post_id| uuid - foreign key (post.id)|
| user_id| uuid - foreign key (user.id)|

### **Hashtag_posts** ⬜

|**column**| *definition*                   |
|---------|------------------------------|
| id      | uuid                         |
| post_id | uuid - foreign key (post.id) |
| user_id | uuid - foreign key (user.id) |

### **User_following** ⬜

|**column** | *definition*                 |
|-----------|------------------------------|
|id         | uuid                         |
|follower_id| uuid - foreign key (user.id) |
|followee_id| uuid - foreign key (user.id) |

---

![ER Diagram](https://user-images.githubusercontent.com/13291222/121964106-a573ad00-cd88-11eb-8510-dfac87552568.png)

---

## API

### Auth 🔐

⬜ **POST /auth/login**
> Login and get auth token
<br\>

### Users 👤

⬜ **GET /users/@{username}**
> Get details of given user by username

⬜ **GET /users/{userid}**
> Get details of given user by userid

⬜ **POST /users**
> Create a new user

⬜ **PATCH /users/{userid} 🔒**
> Update bio/name/image etc of an user

⬜ **PUT /users/{userid}/follow 🔒**
> Follow the given user

⬜ **DELETE /users/{userid}/follow 🔒**
> Un-follow the given user

⬜ **GET /users/{userid}/followers 📃**
> Get a list of all followers of this user

⬜ **GET /users/{userid}/followees 📃**
> Get a list of all followees of this user

## Posts [not like *REST* but like **Tweets**]

### **Post structure**

> Version 1

The latest schema can be found [here](json-schema/posts.json). Better examples can be found [here](json-schema/posts.json). Please refer to the tailing part of the schema as they are the examples generated through the schema itself.

```json V1
{
 "id": 1234567890,
 "text": "Raw tweet text",  // String including @ and #, supports emoji
 "user": 32524515,          // User id of posting user
 "like_count": 100,
 "repliy_count": 10,
 "view_count": 1000,
 "in_reply_to": 1234567890, // can be null or id of tweet
 "share_count": 10,
 "timestamp": 1674898651,   // Timestamp of post, in epoch seconds
 "replies": [{              // Same structure as post, without in_reply_to
   "id": 1234567890,
   "text": "Raw tweet text",
   "like_count": 50,
   "repliy_count": 10,
   "user": 245610214,
   "view_count": 1000,
   "share_count": 10,
   "timestamp": 1674898651
  },
  {
   "id": 1234567890,
   "text": "Raw tweet text",
   "like_count": 50,
   "repliy_count": 10,
   "user": 51247536,
   "view_count": 1000,
   "share_count": 10,
   "timestamp": 1674898651
  }
 ],
  "replies_pages": {
    "prev": "?page=1&limit=50",
    "next": "?page=3&limit=50"
  }
}
```

> Version 2 \
> // TODO ⬜

⬜ [**GET /posts 📃**](hld/get_posts.md)
> Either Empty or takes one of the following parameters

| **query** | *definition*|
|----------|------------|
| -*empty*-| the authorised user [the person who is logged in]            |
| by       | username   |
| reply_to | post uuid  |
| hashtag  | tag        |

⬜ **GET /posts/{postid}**
> Get details of a post

⬜ **PUT /posts/{postid}/like 🔒**
> Like the given post [having id as *postid*]

### Hashtags \#

⬜ **GET /hashtags 📃**
> Top hashtags (default top 10)

⬜ **GET /hashtags/{tag}/posts 📃**
> All posts of this given hashtag

---

## Projects

- [Backend](https://github.com/scaleracademy/twitter-backend-node) (in NodeJS)
- [Backend](https://github.com/scaleracademy/twitter-backend-java) (in Java)
