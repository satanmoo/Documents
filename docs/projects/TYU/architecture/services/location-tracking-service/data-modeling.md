# Data modeling

## user location data

```text
CREATE TABLE user_location (
    location_id SERIAL PRIMARY KEY,
    user_id INT NOT NULL REFERENCES users (user_id),
    latitude FLOAT8 NOT NULL,
    longitude FLOAT8 NOT NULL,
    nx SMALLINT NOT NULL,
    ny SMALLINT NOT NULL,
    human_address VARCHAR(255) NOT NULL,
    visit_count BIGINT DEFAULT 1,
    last_visited_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    
    UNIQUE(user_id, nx, ny)
);
```

### index

```text
CREATE INDEX idx_user_locations_user_id ON user_locations(user_id);
```
