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

7. Photos & Hashtags

CREATE TABLE photo_tags (
    photo_id INTEGER NOT NULL,
    tag_id INTEGER NOT NULL,
    FOREIGN KEY(photo_id) REFERENCES photos(id),
    FOREIGN KEY(tag_id) REFERENCES tags(id),
    PRIMARY KEY(photo_id, tag_id)
);

![image](https://user-images.githubusercontent.com/121452974/209835857-4fab5d45-f214-4c84-8d5f-7c3bf58518c2.png)















































