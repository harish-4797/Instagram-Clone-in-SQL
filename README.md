# Instagram-Clone-in-SQL

## Project Overview

This project is a simulation of an Instagram-like database system, designed to manage user interactions, photo sharing, likes, comments, hashtags, and follows. It includes advanced SQL queries for analytical insights, like finding the top hashtags and identifying bot-like behavior.

## Database Schema

The database contains the following tables:

* users- Stores user information.
* photos - Stores photo URLs and associated user IDs.
* comments - Manages comments on photos.
* likes - Records likes on photos by users.
* follows - Tracks user follow relationships.
* tags - Manages hashtag names.
* photo\_tags - Connects tags to photos.

## SQL Queries

Below are some important SQL queries executed in the project:

### 1. Top 5 Most Commonly Used Hashtags

```sql
SELECT t.tag_name, COUNT(pt.tag_id) AS usage_count
FROM tags t
JOIN photo_tags pt ON t.id = pt.tag_id
GROUP BY t.tag_name
ORDER BY usage_count DESC
LIMIT 5;
```

### 2. Number of Users Who Have Liked Every Single Photo

```sql
SELECT COUNT(user_id) AS total_users
FROM likes l
GROUP BY l.user_id
HAVING COUNT(photo_id) = (SELECT COUNT(*) FROM photos);
```

### 3. Find Users with Most Followers

```sql
SELECT u.username, COUNT(f.follower_id) AS followers
FROM users u
JOIN follows f ON u.id = f.followee_id
GROUP BY u.username
ORDER BY followers DESC
LIMIT 5;
```

### 4. Most Liked Photos

```sql
SELECT p.image_url, COUNT(l.user_id) AS like_count
FROM photos p
JOIN likes l ON p.id = l.photo_id
GROUP BY p.image_url
ORDER BY like_count DESC
LIMIT 5;
```

### 5. Average Posts Per User

```sql
SELECT 
    ROUND((SELECT COUNT(*) FROM photos) / (SELECT COUNT(*) FROM users)) AS avg_user_post;
```

### 6. 5 Oldest Users

```sql
SELECT * FROM users
ORDER BY created_at
LIMIT 5;
```

### 7. Most Common Day for User Registration

```sql
SELECT
    DAYNAME(created_at) AS dayname,
    COUNT(*)
FROM users
GROUP BY dayname
ORDER BY COUNT(*) DESC
LIMIT 1;
```

### 8. Inactive Users (Never Posted a Photo)

```sql
SELECT users.username
FROM users
LEFT JOIN photos
ON users.id = photos.user_id
WHERE photos.image_url IS NULL;
```

### 9. User with Most Likes on a Single Photo

```sql
SELECT u.username, p.image_url, COUNT(*) AS like_count
FROM photos p
JOIN likes l ON p.id = l.photo_id
JOIN users u ON u.id = p.user_id
GROUP BY p.id
ORDER BY like_count DESC
LIMIT 1;
```

## Setup Instructions

1. Clone the repository:

   ```bash
   git clone <repository-url>
   ```
2. Navigate to the project directory:

   ```bash
   cd instagram-clone-db
   ```
3. Import the SQL script to your MySQL database:

   ```sql
   SOURCE path_to_your_sql_file.sql;
   ```
4. Use the database:

   ```sql
   USE instagram_clone_db;
   ```

## Technologies Used

* MySQL
* SQL Queries
* Database Management

## Future Enhancements

* Implement stored procedures for optimized data handling.
* Integrate triggers for automated tasks like updating follower counts.
* Advanced analytics for hashtag trends over time.
