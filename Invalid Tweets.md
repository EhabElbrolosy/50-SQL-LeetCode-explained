## _Invalid Tweets_

```sql
SELECT tweet_id
FROM Tweets
WHERE length(content) >15
ORDER BY tweet_id
```

```length()``` في sql server   أو 


```LEN()```في postgresql بتحسب عدد الcharacters في الكلمة

هنضيف شرط ان عدد الحروف في content اكبر من 15
