# SQL CRUD Report

## Part One:
### **Firstly, creating tables: **restaurants** and **reviews**. The codes are listed below:**
```sql
CREATE TABLE Restaurants (
    RestaurantID INTEGER PRIMARY KEY AUTOINCREMENT,
    Name TEXT NOT NULL,
    Category TEXT NOT NULL
    PriceTier TEXT NOT NULL CHECK (PriceTier IN ('cheap','medium','expensive')),
    Neighborhood TEXT NOT NULL,
    OpeningHours TEXT NOT NULL,
    ClosingHours TEXT NOT NULL,
    AverageRating REAL CHECK (AverageRating >= 0 AND AverageRating <= 5>),
    GoodForKids INTEGER NOT NULL CHECK (GoodForKids IN (0,1)) -- 0 for false, 1 for true
);
CREATE TABLE IF NOT EXISTS Reviews (
    ReviewID INTEGER PRIMARY KEY AUTOINCREMENT,
    RestaurantID INTEGER,
    ReviewText TEXT,
    ReviewDate TIMESTAMP DEFAULT CURRWNT_TIMESTAMP,
    FOREIGN KEY (RestaurantID) REFEREMCES Restaurant(RestaurantID)
);
```
### **Secondly, importing data retrieved from [Mockaroo](mockaroo.com) into the two tables I just created:**
```sql
.import ""C:\\Users\\16701\\Downloads\\restaurants.csv" Restaurants;
```
### **Queries**
1. Find all cheap restaurants in a particular neighborhood (pick any neighborhood as an example).
```sql 
select * from Restaurants where PriceTier='cheap' and Neighborhood='Bronx";
```
2. Find all restaurants in a particular genre (pick any genre as an example) with 3 stars or more, ordered by the number of stars in descending order.
```sql
select * from Restaurants where Category="Chinese' and AverageRating>=3 order by AverageRating DESC;
```
3. Find all restaurants that are open now (see hint below).
```sql
select * from Restaurants where OpeningHours,=strftime('%H:%M', 'now','localtime') and ClosingHours>=strftime('%H:%M', 'now','localtime');
```
4. Leave a review for a restaurant (pick any restaurant as an example; note that leaving a review has no automatic effect on the average rating of the restaurant).
```sql
insert into Reviews (Restaurant,ReviewText,ReviewDate) values (1,'god','2024/24');
```
5. Delete all restaurants that are not good for kids.
```sql
DELETE FROM Restaurants
```
6. Find the number of restaurants in each NYC neighborhood.
```sql
Neighborhood, COUNT(*) AS NumaberOfRestaurants FROM Restaurants Group BY Neighborhood;
```

## Part Two:
### Creating **Users** and **Posts**:
```sql
CREATE TABLE Users (
    UserID INTEGER PRIMARY KEY AUTOINCREMENT,
    Email TEXT NOT NULL,
    Password TEXT NOT NULL,
    Handle TEXT NOT NULL
);
CREATE TABLE Posts (
    PostID INTEGER PRIMARY KEY AUTOINCREMENT,
    UserID INTEGER NOT NULL,
    PostType TEXT NOT NULL CHECK (PostType IN ('Message','Story')),
    Content TEXT NOT NULL,
    ReceivedID INTEGER,
    PostTime TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    ExpiredTime TIMESTAMP,
    FOREIGN KEY (UserID) REFERENCES Users(UserID),
    FOREIGN KEY (ReceivedID) REFERENCES Users(UserID)
);
```
### Impoering data into Users and Posts:
```sql
.import "C:\\Users\\16701\\Downloads\\UsersData.csv' Users
.import "C:\\Users\\16701\\Downloads\\MessageData.csv" Posts
.import "C:\\Users\\16701\\Downloads\\StoryData.csv" Posts
```

### Queries
1. Show the email addresses of all Users who have not posted anything yet.
```sql
insert into Users (Email, Password,Handle) VALUES ('qwedsazxc@gmail.com','qwedsazxc','qwe');
```
2. Create a new Message sent by a particular User to a particular User (pick any two Users for example).
```sql
insert into Posts (UserID, PostType, Content, ReceivedID,ExpiredTime) VALUES (1, 'Message','qqqqqq',2, CURRENT_TIMESTAMP, datetime('now','+2 minutes'));
```
3. Create a new Story by a particular User (pick any User for example).
```sql
insert into Posts (UserID, PostType, Content, ReceivedID,ExpiredTime) VALUES (1, 'Story','qqqqqq',, CURRENT_TIMESTAMP, datetime('now','+1448 minutes'));
```
4. Show the 10 most recent visible Messages and Stories, in order of recency.
```sql
select * FROM Posts where ExpiredTime>CURRENT_TIMESTAMP order by PostTIme DESC limit 10;
```
5. Show the 10 most recent visible Messages sent by a particular User to a particular User (pick any two Users for example), in order of recency.
```sql
select * FROM Posts where PostType='Message' and UserID=1 and ReceivedID=2 and ExpiredTime>CURRENT_TIMESTAMP order by PostTime DESC limit 10;
```
6. Make all Stories that are more than 24 hours old invisible.
```sql
UPDATE Posts SET ExpiredTime = CURRENT_TIMESTAMP WHERE PostType = 'Story' AND PostTime <= datatime('now','-24houurs');
```
**Tips**: ExpiredTime here stands for the moment that a post starts to be invisble

7. Show all invisible Messages and Stories, in order of recency.
```sql
select * FROM Posts where ExpiredTime < CURRENT_TIMESTAMP order by PostTIme DESC;
```
8. Show the number of posts by each User.
```sql
select UserID, COUNT(*) AS NumberOfPosts FROM Posts GROUP BY UserID;
```
9. Show the post text and email address of all posts and the User who made them within the last 24 hours.
```sql
select Posts.Content, Users.Email From Users JOIN Posts on Users.UserID=Posts.UserID where PostTime>=datatime('now','-24 hours');
```

10. Show the email addresses of all Users who have not posted anything yet.
```sql
select Users.Email From Users LEFT JOIN Posts on Users.UserID where Posts.PostID is NULL;
```
### Links to all the files:
- [MessageData](https://github.com/dbdesign-students-spring2024/4-sql-crud-NOLIMIT0410/blob/main/data/MessageData.csv)
- [StoryData](https://github.com/dbdesign-students-spring2024/4-sql-crud-NOLIMIT0410/blob/main/data/StoryData.csv)
- [UsersDatya](https://github.com/dbdesign-students-spring2024/4-sql-crud-NOLIMIT0410/blob/main/data/UsersData.csv)
- [restaurants](https://github.com/dbdesign-students-spring2024/4-sql-crud-NOLIMIT0410/blob/main/data/restaurants.csv)
