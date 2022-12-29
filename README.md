# IG_Clone

The Instagram Clone Project in My SQL.

The project is based on the Instagram model, where the Instagram database was cloned in a simple way, with 7 tables containing basic Instagram data, such as users, photos, likes, comments, hashtags, etc.

The basic idea behind this project is to create appropriate tables, insert some data and perform some of the most common SQL operations on that data – i.e. data analysis. 

Data was acquired through an online MySQL course and inserted into tables accordingly.

As mentioned before, the first step is to create the necessary tables:

1.	Users

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
); 

![image](https://user-images.githubusercontent.com/121452974/209833556-d1370880-4c66-4cc3-95a3-523b286f6339.png)

2. Photos 

CREATE TABLE photos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    image_url VARCHAR(255) NOT NULL,
    user_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(user_id) REFERENCES users(id)
);

![image](https://user-images.githubusercontent.com/121452974/209834128-5d46d756-12ab-4107-a54d-95d2bcad9f37.png)

3. Comments

CREATE TABLE comments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    comment_text VARCHAR(255) NOT NULL,
    photo_id INT NOT NULL,
    user_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    FOREIGN KEY(user_id) REFERENCES users(id)
);

![image](https://user-images.githubusercontent.com/121452974/209834275-3adc4115-5e2d-46f3-86c5-4c360bf24454.png)

4. Likes

CREATE TABLE likes (
    user_id INT NOT NULL,
    photo_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(user_id) REFERENCES users(id),
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    PRIMARY KEY(user_id, photo_id)
);

![image](https://user-images.githubusercontent.com/121452974/209834438-4034832e-655a-4a70-98ed-945d8862e31e.png)

5. Follows

CREATE TABLE follows (
    follower_id INT NOT NULL,
    followee_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(follower_id) REFERENCES users(id),
    FOREIGN KEY(followee_id) REFERENCES users(id),
    PRIMARY KEY(follower_id, followee_id)
);

![image](https://user-images.githubusercontent.com/121452974/209834520-ac1bf909-4121-4f3a-ba00-c383405a0959.png)

6. Hashtags

CREATE TABLE tags (
  id INT AUTO_INCREMENT PRIMARY KEY,
  tag_name VARCHAR(255) UNIQUE,
  created_at TIMESTAMP DEFAULT NOW()
);

![image](https://user-images.githubusercontent.com/121452974/209835602-59ae1891-53bd-4f76-a1d8-380fd14aa5f4.png)

7. Hashtags on Photos

CREATE TABLE photo_tags (
    photo_id INTEGER NOT NULL,
    tag_id INTEGER NOT NULL,
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    FOREIGN KEY(tag_id) REFERENCES tags(id),
    PRIMARY KEY(photo_id, tag_id)
);

![image](https://user-images.githubusercontent.com/121452974/209835857-4fab5d45-f214-4c84-8d5f-7c3bf58518c2.png)



After creating the necessary tables that will store the basic Instagram data and inserting data that was acquired online, it is time to do some analytics with the most commonly used SQL functions.



# TASKS


-- 1. Finding 5 oldest users

SELECT * FROM users
     ORDER BY created_at
     LIMIT 5;

![image](https://user-images.githubusercontent.com/121452974/209992396-fed781e7-053c-4a1b-bfd0-aecd29787173.png)




-- 2. Most Popular Registration Date (Day)

SELECT 
    DAYNAME(created_at) AS Day, COUNT(username) AS Total
FROM
    users
GROUP BY Day
ORDER BY Total DESC
LIMIT 1;

![image](https://user-images.githubusercontent.com/121452974/209992455-8ff89b70-c70c-4ca9-b024-0350827b55ac.png)




-- 3. Identify Inactive Users (users with no photo)

SELECT 
    username, IFNULL(photos.id, 'No Photo') AS Activity
FROM
    users
        LEFT JOIN
    photos ON users.id = photos.user_id
WHERE
    photos.id IS NULL;
    
![image](https://user-images.githubusercontent.com/121452974/209992770-8dda8a91-8ed0-418b-80c0-00bb5cbccd1b.png)




    -- 4. Identify most popular photo (and user who created it)

SELECT 
    username,
    photos.id,
    image_url,
    COUNT(likes.user_id) AS likes
FROM
    photos
        JOIN
    likes ON likes.photo_id = photos.id
        JOIN
    users ON users.id = photos.user_id
GROUP BY likes.photo_id
ORDER BY likes DESC
LIMIT 1;

![image](https://user-images.githubusercontent.com/121452974/209992830-fca90ddc-870a-4dc5-8f21-ea85c9495eb0.png)




-- 5. How many times does the average user posts?

-- total number of photos / total number of users

SELECT 
      (SELECT COUNT(*) FROM photos) / (SELECT COUNT(*) FROM users) AS Average_posts;
      
      
![image](https://user-images.githubusercontent.com/121452974/209992958-8a911419-abe4-4487-baaa-35e16316582c.png)




-- 6. Find the five most commonly used hashtags

SELECT 
    tag_name, COUNT(*) AS Total
FROM
    tags
        JOIN
    photo_tags ON tags.id = photo_tags.tag_id
GROUP BY tag_name
ORDER BY Total DESC
LIMIT 5;

![image](https://user-images.githubusercontent.com/121452974/209993072-f81e3165-8f8d-4c16-a4f3-0bab57ffebb6.png)




-- 7. Find users who have liked every single photo on the site (Potential Bots)
 
SELECT 
    username, COUNT(*) AS num_likes
FROM
    users
        JOIN
    likes ON likes.user_id = users.id
GROUP BY likes.user_id
HAVING num_likes = (SELECT 
        COUNT(*)
    FROM
        photos);
        
![image](https://user-images.githubusercontent.com/121452974/209993374-78e39231-6515-4579-b1e1-84e597656522.png)




-- 8. Find the TOP 5 most popular users (with the most followers)

SELECT 
    followee_id, username, COUNT(F.follower_id) AS Followers
FROM
    follows AS F
        JOIN
    users ON users.id = F.followee_id
GROUP BY F.followee_id
ORDER BY Followers DESC
LIMIT 5;


![image](https://user-images.githubusercontent.com/121452974/209993433-e04d3616-5628-48ad-a449-8450b66d9b6d.png)




-- 9. Since my IG username is Emir_Masovic if would like to se all users that have similiar username, or the username that starts with 'Em' or 'Ma'

SELECT 
    username
FROM
    users
WHERE
    username LIKE 'Emir'
        OR username LIKE '%Emir%'
        OR username LIKE 'Em%'
        OR username LIKE 'Ma%';

![image](https://user-images.githubusercontent.com/121452974/209993498-bd647b8a-0dc7-4f5c-8470-bc70b094163f.png)





Thanks!

Emir.





























