# sql-ig_clone_data
My SQL queries for this Database ig_clone_data
/* 1. Create an ER diagram or draw a schema for the given database.
 
 
 
2. We want to reward the user who has been around the longest, Find the 5 oldest users. */
 

 SELECT * FROM users
ORDER BY created_at
LIMIT 5;
 
-- 3. To target inactive users in an email ad campaign, find the users who have never posted a photo.
SELECT username
FROM users
LEFT JOIN photos ON users.id = photos.user_id
WHERE photos.id IS NULL;

-- 4. Suppose you are running a contest to find out who got the most likes on a photo. Find out who won?

SELECT 
    username,
    photos.id,
    photos.image_url, 
    COUNT(*) AS total
FROM photos
INNER JOIN likes
    ON likes.photo_id = photos.id
INNER JOIN users
    ON photos.user_id = users.id
GROUP BY photos.id
ORDER BY total DESC
LIMIT 1;



-- 5. The investors want to know how many times does the average user post.

SELECT ROUND((SELECT COUNT(*)FROM photos)/(SELECT COUNT(*) FROM users),2);



-- 6. A brand wants to know which hashtag to use on a post, and find the top 5 most used hashtags.
SELECT tag_name, COUNT(tag_name) AS total
FROM tags
JOIN photo_tags ON tags.id = photo_tags.tag_id
GROUP BY tags.id
ORDER BY total DESC limit 5;



-- 7. To find out if there are bots, find users who have liked every single photo on the site.
SELECT users.id,username, COUNT(users.id) As total_likes_by_user
FROM users
JOIN likes ON users.id = likes.user_id
GROUP BY users.id
HAVING total_likes_by_user = (SELECT COUNT(*) FROM photos);



-- 8. Find the users who have created instagramid in may and select top 5 newest joinees from it?

SELECT * from users where month(created_at) = 5 order by month(created_at) desc limit 5;




-- 9. Can you help me find the users whose name starts with c and ends with any number and have posted the photos as well as liked the photos?
-- find the users whose name starts with c and ends with any number
-- and have posted photos and 
-- liked the photos

select  * from users u 
INNER JOIN likes l
    ON l.user_id = u.id
INNER JOIN photos p
    ON p.user_id = u.id
where  u.username regexp '^c' and  u.username regexp '[0-9]$' ;

-- 10. Demonstrate the top 30 usernames to the company who have posted photos in the range of 3 to 5.

select username ,count(p.user_id) from users u INNER JOIN photos p
    ON p.user_id = u.id group by p.user_id  having count(p.user_id) between 3 and 5;
