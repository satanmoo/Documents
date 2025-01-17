# Data modeling

## user location data

```shell
CREATE TABLE user_location_data
(
    user_id         INT NOT NULL,        -- Unique user identifier
    ny              INT NOT NULL,        -- Grid (ny) coordinate
    nx              INT NOT NULL,        -- Grid (nx) coordinate
    address         VARCHAR(255),        -- Readable address corresponding to the (ny, nx) grid
    dwell_time      INT NOT NULL,        -- Dwell time in seconds (e.g., total time spent in this grid)
    last_visited    TIMESTAMP NOT NULL,  -- Timestamp of the last visit
    PRIMARY KEY (user_id, ny, nx)       -- Primary key to ensure uniqueness of user-location combination
);
```

## grid address data

- reveres geocoding 결과

```sql
CREATE TABLE grid_to_address
(
    ny      INT          NOT NULL, -- Grid (ny) coordinate
    nx      INT          NOT NULL, -- Grid (nx) coordinate
    address VARCHAR(255) NOT NULL, -- Readable address corresponding to the grid
    PRIMARY KEY (ny, nx)           -- Primary key on the grid coordinates
);
```
