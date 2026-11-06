---
topic: Heap
difficulty: Medium
leetcode: https://leetcode.com/problems/design-twitter
---
# Design Twitter

[[Topic Wise Summary|← Back to summary]] · [LeetCode](https://leetcode.com/problems/design-twitter)

> Per-user tweet lists + follow set. Merge recent tweets with a size-10 min-heap.

## Problem

Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and is able to see the 10 most recent tweets in the user's news feed.

Implement the Twitter class:

Twitter() Initializes your twitter object.
void postTweet(int userId, int tweetId) Composes a new tweet with ID tweetId by the user userId. Each call to this function will be made with a unique tweetId.
List<Integer> getNewsFeed(int userId) Retrieves the 10 most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themself. Tweets must be ordered from most recent to least recent.
void follow(int followerId, int followeeId) The user with ID followerId started following the user with ID followeeId.
void unfollow(int followerId, int followeeId) The user with ID followerId started unfollowing the user with ID followeeId.

## Notes

use a feed heap for each user and maintain a list of followers for each user.

## Complexity

getNewsFeed; Flog(k)

F: number of followers; heap pushes 10 times for each F

## Code

```cpp
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> top_posts_for_feed;

        //first let;s go thorugh the user's posts
        vector<PostTimestamp> user_posts = user_post_data[userId];
        for(int i = user_posts.size() - 1; i >= max(0, (int)user_posts.size() - TOP_TEN); --i) {
            top_posts_for_feed.push({user_posts[i].ts, user_posts[i].post_id});
        }

        //then one by one from the follower's list
        vector<int> follower_list_for_user = vector<int>(follower_list[userId].begin(), follower_list[userId].end());

        for(int followee_id: follower_list_for_user) {
            vector<PostTimestamp> followee_posts = user_post_data[followee_id];
            for(int i = followee_posts.size() - 1; i >= max(0, (int)followee_posts.size() - TOP_TEN); --i) {
                top_posts_for_feed.push({followee_posts[i].ts, followee_posts[i].post_id});
                while(top_posts_for_feed.size() > 10) top_posts_for_feed.pop();
            }
        }

        //now get the elements from the heap
        vector<int> res(top_posts_for_feed.size());
        int position = res.size() - 1;

        while(!top_posts_for_feed.empty()) {
            res[position--] = top_posts_for_feed.top().second;
            top_posts_for_feed.pop();
        }

        return res;
```
