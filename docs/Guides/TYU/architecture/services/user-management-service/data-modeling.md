# Data modeling

```shell
CREATE TABLE user_data
(
    user_id          INT NOT NULL,         -- Unique user identifier (system-generated ID)
    social_login_id  VARCHAR(255),         -- User's ID from the social login platform (e.g., Google, Facebook)
    user_time        TIME NOT NULL,        -- User-defined time for notification preferences (e.g., 14:00)
    nickname         VARCHAR(100),         -- User's nickname
    PRIMARY KEY (user_id)                  -- Primary key to uniquely identify each user
);
```
